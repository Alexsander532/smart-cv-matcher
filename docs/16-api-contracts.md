# 16 — Contratos de API e Arquitetura Desacoplada

## Visão Geral

Este documento define os contratos de API REST para o Smart CV Matcher, com foco em uma arquitetura desacoplada que permite alternar entre diferentes fontes de dados (mock com JSON local, banco de dados real, ou outras fontes) sem alterações no código da aplicação.

## Arquitetura de Camadas

```
┌─────────────────────────────────────────────────────────────┐
│                         Frontend                             │
│                  (React/Vue/Angular)                         │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                      API Layer                               │
│           (Controllers/Routes/Handlers)                      │
│    ┌──────────────────────────────────────────────────┐     │
│    │         Contratos de API (Este Documento)        │     │
│    └──────────────────────────────────────────────────┘     │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    Service Layer                             │
│              (Business Logic / Rules)                        │
│    ┌──────────────────────────────────────────────────┐     │
│    │           Motor de Pontuação / Validação         │     │
│    └──────────────────────────────────────────────────┘     │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                  Repository Layer                            │
│              (Data Access Interface)                         │
│    ┌──────────────────────────────────────────────────┐     │
│    │        Interfaces: IProfileRepository            │     │
│    │                   ICriteriaRepository            │     │
│    │                   IScoringRepository             │     │
│    └──────────────────────────────────────────────────┘     │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
        ┌──────────┐   ┌──────────┐   ┌──────────┐
        │   Mock   │   │ Database │   │   API    │
        │ (JSON)   │   │ (Prisma/ │   │ Externa  │
        │          │   │  TypeORM)│   │          │
        └──────────┘   └──────────┘   └──────────┘
```

## Padrão Repository

### Interfaces

```typescript
// Exemplo em TypeScript - Interfaces do Repository

export interface IProfileRepository {
  findAll(filters?: ProfileFilters): Promise<Profile[]>;
  findById(id: string): Promise<Profile | null>;
  create(profile: CreateProfileDTO): Promise<Profile>;
  update(id: string, profile: UpdateProfileDTO): Promise<Profile>;
  delete(id: string): Promise<void>;
}

export interface ICriteriaRepository {
  findByOpportunityId(opportunityId: string): Promise<CriteriaSetVersion>;
  createVersion(version: CreateCriteriaVersionDTO): Promise<CriteriaSetVersion>;
  approveVersion(versionId: string): Promise<CriteriaSetVersion>;
}

export interface IScoringRepository {
  execute(run: ScoringRunDTO): Promise<ScoringRun>;
  getResults(runId: string): Promise<ScoringResult>;
  getTeamAssignments(runId: string): Promise<TeamAssignment[]>;
}

export interface IOpportunityRepository {
  findAll(filters?: OpportunityFilters): Promise<Opportunity[]>;
  findById(id: string): Promise<Opportunity | null>;
  create(opportunity: CreateOpportunityDTO): Promise<Opportunity>;
  update(id: string, opportunity: UpdateOpportunityDTO): Promise<Opportunity>;
}
```

### Implementação Mock (JSON)

```typescript
// repositories/MockProfileRepository.ts
import profilesData from '../data/perfis-demonstracao.json';
import { IProfileRepository, Profile, ProfileFilters } from '../interfaces';

export class MockProfileRepository implements IProfileRepository {
  private profiles: Profile[] = profilesData.profiles;

  async findAll(filters?: ProfileFilters): Promise<Profile[]> {
    let result = [...this.profiles];
    
    if (filters?.available !== undefined) {
      result = result.filter(p => p.available === filters.available);
    }
    
    if (filters?.pmpValid !== undefined) {
      result = result.filter(p => {
        if (!p.pmp) return !filters.pmpValid;
        const validUntil = new Date(p.pmp.valid_until);
        const cutoff = filters.cutoffDate ? new Date(filters.cutoffDate) : new Date();
        return filters.pmpValid ? validUntil >= cutoff : validUntil < cutoff;
      });
    }
    
    return result;
  }

  async findById(id: string): Promise<Profile | null> {
    return this.profiles.find(p => p.id === id) || null;
  }

  async create(profile: CreateProfileDTO): Promise<Profile> {
    const newProfile = {
      id: `P${String(this.profiles.length + 1).padStart(3, '0')}`,
      ...profile,
      synthetic: true,
      profile_reviewed_on: new Date().toISOString().split('T')[0],
    };
    this.profiles.push(newProfile);
    return newProfile;
  }

  async update(id: string, profile: UpdateProfileDTO): Promise<Profile> {
    const index = this.profiles.findIndex(p => p.id === id);
    if (index === -1) throw new Error('Profile not found');
    this.profiles[index] = { ...this.profiles[index], ...profile };
    return this.profiles[index];
  }

  async delete(id: string): Promise<void> {
    const index = this.profiles.findIndex(p => p.id === id);
    if (index !== -1) this.profiles.splice(index, 1);
  }
}
```

### Injeção de Dependência

```typescript
// config/repositoryConfig.ts
import { IProfileRepository } from '../interfaces';
import { MockProfileRepository } from '../repositories/MockProfileRepository';
// import { DatabaseProfileRepository } from '../repositories/DatabaseProfileRepository';

export function configureRepositories(): {
  profileRepository: IProfileRepository;
  // ... outros repositories
} {
  const useMock = process.env.DATA_SOURCE === 'mock' || true; // Default mock para PoC
  
  return {
    profileRepository: useMock 
      ? new MockProfileRepository() 
      : new DatabaseProfileRepository(), // Implementação futura
  };
}
```

## Contratos de API REST

### Base URL

```
Desenvolvimento: http://localhost:3001/api/v1
Produção: https://api.smartcv.example.com/api/v1
```

### Autenticação

Todas as requisições (exceto endpoints públicos) devem incluir header:

```
Authorization: Bearer <token>
```

Para a PoC com mock, a autenticação pode ser simulada.

---

## Endpoints

### 1. Profissionais (Profiles)

#### GET /professionals

Lista profissionais com filtros opcionais.

**Query Parameters:**

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| `available` | boolean | Não | Filtra por disponibilidade |
| `pmp_valid` | boolean | Não | Filtra por PMP válida |
| `cutoff_date` | string (YYYY-MM-DD) | Não | Data de corte para validade |
| `page` | integer | Não | Página (default: 1) |
| `limit` | integer | Não | Itens por página (default: 20, max: 100) |

**Response 200:**

```json
{
  "data": [
    {
      "id": "P001",
      "display_name": "Ana Exemplo",
      "available": true,
      "availability_period": {
        "start": "2026-11-01",
        "end": "2027-04-30"
      },
      "pmp": {
        "name": "PMP",
        "valid_until": "2027-12-31",
        "evidence": {
          "status": "accepted",
          "reviewed_on": "2026-10-21"
        }
      },
      "postgraduate": {
        "name": "Pós-graduação em Gestão de Projetos de Energia",
        "completed": true,
        "relevance_approved": true
      },
      "energy_projects_count": 4,
      "career_years": 16.8,
      "profile_reviewed_on": "2026-10-21"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 12,
    "total_pages": 1
  }
}
```

#### GET /professionals/{id}

Retorna detalhes de um profissional específico.

**Response 200:**

```json
{
  "id": "P001",
  "display_name": "Ana Exemplo",
  "synthetic": true,
  "available": true,
  "availability_period": {
    "start": "2026-11-01",
    "end": "2027-04-30"
  },
  "pmp": {
    "name": "PMP",
    "valid_until": "2027-12-31",
    "evidence": {
      "synthetic": true,
      "reference": "synthetic:P001:pmp",
      "status": "accepted",
      "reviewed_on": "2026-10-21"
    }
  },
  "postgraduate": {
    "name": "Pós-graduação em Gestão de Projetos de Energia",
    "completed": true,
    "relevance_approved": true,
    "evidence": {
      "synthetic": true,
      "reference": "synthetic:P001:postgraduate",
      "status": "accepted",
      "reviewed_on": "2026-10-21"
    }
  },
  "energy_projects": [
    {
      "canonical_project_id": "P001-ENERGIA-01",
      "name": "Projeto de energia 1",
      "sector": "energia",
      "evidence": {
        "status": "accepted",
        "reviewed_on": "2026-10-21"
      }
    }
  ],
  "career_periods": [
    {
      "start_date": "2010-01-01",
      "end_date": "2026-10-21",
      "evidence": {
        "status": "accepted",
        "reviewed_on": "2026-10-21"
      }
    }
  ],
  "profile_reviewed_on": "2026-10-21"
}
```

**Response 404:**

```json
{
  "error": {
    "code": "PROFESSIONAL_NOT_FOUND",
    "message": "Professional with id P999 not found"
  }
}
```

#### POST /professionals

Cria um novo profissional.

**Request Body:**

```json
{
  "display_name": "Maria Silva",
  "available": true,
  "availability_period": {
    "start": "2026-11-01",
    "end": "2027-04-30"
  },
  "pmp": {
    "name": "PMP",
    "valid_until": "2027-12-31"
  },
  "postgraduate": {
    "name": "MBA em Gestão de Projetos",
    "completed": true,
    "relevance_approved": true
  },
  "energy_projects": [
    {
      "name": "Usina Solar Fotovoltaica",
      "sector": "energia"
    }
  ],
  "career_periods": [
    {
      "start_date": "2015-01-01",
      "end_date": "2026-10-21"
    }
  ]
}
```

**Response 201:**

```json
{
  "id": "P013",
  "display_name": "Maria Silva",
  "available": true,
  "profile_reviewed_on": "2026-10-21"
}
```

#### PATCH /professionals/{id}

Atualiza parcialmente um profissional.

**Request Body:**

```json
{
  "available": false
}
```

**Response 200:**

```json
{
  "id": "P013",
  "display_name": "Maria Silva",
  "available": false,
  "profile_reviewed_on": "2026-10-21"
}
```

#### DELETE /professionals/{id}

Remove um profissional.

**Response 204:** No content

---

### 2. Oportunidades (Opportunities)

#### GET /opportunities

Lista oportunidades/disputas.

**Response 200:**

```json
{
  "data": [
    {
      "id": "DEMO-001",
      "title": "Disputa Demonstração",
      "cutoff_date": "2026-10-21",
      "status": "active",
      "roles": [
        {
          "id": "gerente-projeto",
          "name": "Gerente de Projeto",
          "slots": 1
        }
      ]
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 1
  }
}
```

#### GET /opportunities/{id}

Retorna detalhes de uma oportunidade com critérios.

**Response 200:**

```json
{
  "id": "DEMO-001",
  "title": "Disputa Demonstração",
  "cutoff_date": "2026-10-21",
  "status": "active",
  "roles": [
    {
      "id": "gerente-projeto",
      "name": "Gerente de Projeto",
      "slots": 1,
      "allow_multiple_roles_per_person": false
    }
  ],
  "eligibility": [
    {
      "id": "E1",
      "field": "available",
      "operator": "equals",
      "value": true
    },
    {
      "id": "E2",
      "field": "career_periods",
      "operator": "union_calendar_years_at_least",
      "value": 5,
      "accepted_evidence_required": true
    }
  ],
  "criteria": [
    {
      "id": "C1",
      "name": "PMP",
      "kind": "binary",
      "field": "pmp",
      "requires_valid_on_cutoff": true,
      "points": 10,
      "cap": 10
    },
    {
      "id": "C2",
      "name": "Projetos de energia",
      "kind": "distinct_count",
      "field": "energy_projects",
      "distinct_key": "canonical_project_id",
      "points_per_unit": 2,
      "cap": 8
    },
    {
      "id": "C3",
      "name": "Pós-graduação na área do objeto",
      "kind": "binary",
      "field": "postgraduate",
      "requires_completed": true,
      "requires_relevance_approval": true,
      "points": 5,
      "cap": 5
    },
    {
      "id": "C4",
      "name": "Mais de dez anos de experiência",
      "kind": "duration",
      "field": "career_periods",
      "operator": "strictly_greater_than_calendar_years",
      "threshold": 10,
      "points": 5,
      "cap": 5
    }
  ],
  "aggregation": {
    "mode": "additive_independent_criteria",
    "maximum": 28
  }
}
```

#### POST /opportunities

Cria uma nova oportunidade.

**Request Body:**

```json
{
  "title": "Nova Disputa",
  "cutoff_date": "2026-12-01",
  "roles": [
    {
      "id": "gerente-projeto",
      "name": "Gerente de Projeto",
      "slots": 1
    }
  ],
  "criteria": [
    {
      "id": "C1",
      "name": "PMP",
      "kind": "binary",
      "field": "pmp",
      "requires_valid_on_cutoff": true,
      "points": 10,
      "cap": 10
    }
  ]
}
```

**Response 201:**

```json
{
  "id": "OPP-002",
  "title": "Nova Disputa",
  "cutoff_date": "2026-12-01",
  "status": "draft"
}
```

---

### 3. Critérios (Criteria)

#### GET /opportunities/{opportunityId}/criteria-versions

Lista versões de critérios de uma oportunidade.

**Response 200:**

```json
{
  "data": [
    {
      "id": "demo-v1",
      "opportunity_id": "DEMO-001",
      "version": 1,
      "status": "approved",
      "approved_by": "admin",
      "approved_on": "2026-10-21"
    }
  ]
}
```

#### POST /opportunities/{opportunityId}/criteria-versions

Cria nova versão de critérios (rascunho).

**Request Body:**

```json
{
  "criteria": [
    {
      "id": "C1",
      "name": "PMP",
      "kind": "binary",
      "field": "pmp",
      "points": 15,
      "cap": 15
    }
  ]
}
```

**Response 201:**

```json
{
  "id": "demo-v2",
  "opportunity_id": "DEMO-001",
  "version": 2,
  "status": "draft"
}
```

#### POST /criteria-versions/{id}/approval

Aprova uma versão de critérios.

**Response 200:**

```json
{
  "id": "demo-v2",
  "status": "approved",
  "approved_by": "admin",
  "approved_on": "2026-10-21"
}
```

---

### 4. Execução de Pontuação (Scoring Runs)

#### POST /opportunities/{opportunityId}/runs

Executa o cálculo de pontuação para uma oportunidade.

**Request Body:**

```json
{
  "criteria_version_id": "demo-v1",
  "cutoff_date": "2026-10-21",
  "professional_ids": ["P001", "P002", "P003"]
}
```

**Response 202:** (Processamento assíncrono)

```json
{
  "run_id": "run-001",
  "status": "processing",
  "message": "Scoring run started. Use GET /runs/run-001 to check status."
}
```

**Response 200:** (Processamento síncrono para PoC)

```json
{
  "run_id": "run-001",
  "opportunity_id": "DEMO-001",
  "criteria_version_id": "demo-v1",
  "cutoff_date": "2026-10-21",
  "status": "completed",
  "executed_on": "2026-10-21T10:30:00Z",
  "summary": {
    "total_professionals": 12,
    "eligible": 10,
    "ineligible": 2,
    "pending": 1
  }
}
```

#### GET /runs/{runId}

Consulta status e resultados de uma execução.

**Response 200:**

```json
{
  "run_id": "run-001",
  "opportunity_id": "DEMO-001",
  "criteria_version_id": "demo-v1",
  "cutoff_date": "2026-10-21",
  "status": "completed",
  "executed_on": "2026-10-21T10:30:00Z"
}
```

#### GET /runs/{runId}/results

Retorna resultados detalhados da execução.

**Query Parameters:**

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| `role_id` | string | Não | Filtra por função específica |
| `eligibility` | string | Não | Filtra por elegibilidade (elegivel, inelegivel, pendente) |
| `sort_by` | string | Não | Campo para ordenação (default: proven_score) |
| `order` | string | Não | Ordem (asc, desc - default: desc) |

**Response 200:**

```json
{
  "run_id": "run-001",
  "criteria_version_id": "demo-v1",
  "cutoff_date": "2026-10-21",
  "results": [
    {
      "rank": 1,
      "professional_id": "P001",
      "professional_name": "Ana Exemplo",
      "role_id": "gerente-projeto",
      "eligibility": "elegivel",
      "proven_score": 28,
      "potential_score_if_pending_accepted": 28,
      "proven_points_by_criterion": {
        "C1": 10,
        "C2": 8,
        "C3": 5,
        "C4": 5
      },
      "additional_pending_points": 0,
      "breakdown": [
        {
          "criterion_id": "C1",
          "criterion_name": "PMP",
          "points": 10,
          "status": "proven",
          "evidence_reference": "synthetic:P001:pmp"
        },
        {
          "criterion_id": "C2",
          "criterion_name": "Projetos de energia",
          "points": 8,
          "status": "proven",
          "details": {
            "count": 4,
            "cap_applied": true,
            "cap_value": 8
          }
        }
      ]
    },
    {
      "rank": 2,
      "professional_id": "P002",
      "professional_name": "Bruno Exemplo",
      "role_id": "gerente-projeto",
      "eligibility": "elegivel",
      "proven_score": 26,
      "potential_score_if_pending_accepted": 26,
      "proven_points_by_criterion": {
        "C1": 10,
        "C2": 6,
        "C3": 5,
        "C4": 5
      },
      "additional_pending_points": 0
    },
    {
      "rank": 4,
      "professional_id": "P004",
      "professional_name": "Diego Exemplo",
      "role_id": "gerente-projeto",
      "eligibility": "inelegivel",
      "proven_score": 28,
      "ineligibility_reasons": [
        {
          "eligibility_id": "E1",
          "field": "available",
          "reason": "Professional is not available"
        }
      ]
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 12
  }
}
```

#### GET /runs/{runId}/results/{professionalId}

Retorna detalhes de um profissional específico em uma execução.

**Response 200:**

```json
{
  "professional_id": "P001",
  "professional_name": "Ana Exemplo",
  "eligibility": "elegivel",
  "proven_score": 28,
  "potential_score_if_pending_accepted": 28,
  "proven_points_by_criterion": {
    "C1": 10,
    "C2": 8,
    "C3": 5,
    "C4": 5
  },
  "breakdown": [
    {
      "criterion_id": "C1",
      "criterion_name": "PMP",
      "kind": "binary",
      "points": 10,
      "cap": 10,
      "status": "proven",
      "evidence": {
        "reference": "synthetic:P001:pmp",
        "status": "accepted",
        "valid_until": "2027-12-31"
      },
      "computation_details": {
        "field_value": {
          "name": "PMP",
          "valid_until": "2027-12-31"
        },
        "cutoff_date": "2026-10-21",
        "is_valid_on_cutoff": true
      }
    },
    {
      "criterion_id": "C2",
      "criterion_name": "Projetos de energia",
      "kind": "distinct_count",
      "points": 8,
      "cap": 8,
      "status": "proven",
      "computation_details": {
        "distinct_count": 4,
        "points_per_unit": 2,
        "raw_points": 8,
        "capped": true,
        "projects": [
          {
            "id": "P001-ENERGIA-01",
            "name": "Projeto sintético de energia 1",
            "evidence_status": "accepted"
          },
          {
            "id": "P001-ENERGIA-02",
            "name": "Projeto sintético de energia 2",
            "evidence_status": "accepted"
          },
          {
            "id": "P001-ENERGIA-03",
            "name": "Projeto sintético de energia 3",
            "evidence_status": "accepted"
          },
          {
            "id": "P001-ENERGIA-04",
            "name": "Projeto sintético de energia 4",
            "evidence_status": "accepted"
          }
        ]
      }
    },
    {
      "criterion_id": "C3",
      "criterion_name": "Pós-graduação na área do objeto",
      "kind": "binary",
      "points": 5,
      "cap": 5,
      "status": "proven",
      "evidence": {
        "reference": "synthetic:P001:postgraduate",
        "status": "accepted"
      },
      "computation_details": {
        "completed": true,
        "relevance_approved": true
      }
    },
    {
      "criterion_id": "C4",
      "criterion_name": "Mais de dez anos de experiência",
      "kind": "duration",
      "points": 5,
      "cap": 5,
      "status": "proven",
      "computation_details": {
        "operator": "strictly_greater_than_calendar_years",
        "threshold": 10,
        "calculated_years": 16.8,
        "periods": [
          {
            "start_date": "2010-01-01",
            "end_date": "2026-10-21",
            "years": 16.8
          }
        ]
      }
    }
  ],
  "eligibility_check": [
    {
      "id": "E1",
      "field": "available",
      "operator": "equals",
      "value": true,
      "actual_value": true,
      "passed": true
    },
    {
      "id": "E2",
      "field": "career_periods",
      "operator": "union_calendar_years_at_least",
      "value": 5,
      "actual_value": 16.8,
      "passed": true
    }
  ]
}
```

---

### 5. Equipe (Team Assignment)

#### POST /runs/{runId}/team

Define a equipe para uma execução.

**Request Body:**

```json
{
  "assignments": [
    {
      "professional_id": "P001",
      "role_id": "gerente-projeto"
    }
  ],
  "approved_by": "admin"
}
```

**Response 200:**

```json
{
  "run_id": "run-001",
  "team": [
    {
      "professional_id": "P001",
      "professional_name": "Ana Exemplo",
      "role_id": "gerente-projeto",
      "role_name": "Gerente de Projeto",
      "approved_by": "admin",
      "approved_on": "2026-10-21T11:00:00Z"
    }
  ],
  "validation": {
    "valid": true,
    "warnings": [],
    "errors": []
  }
}
```

**Response 400:** (Equipe inválida)

```json
{
  "error": {
    "code": "INVALID_TEAM",
    "message": "Team validation failed",
    "details": [
      {
        "type": "duplicate_assignment",
        "message": "Professional P002 cannot be assigned to multiple roles when allow_multiple_roles_per_person is false"
      },
      {
        "type": "ineligible_professional",
        "message": "Professional P004 is ineligible and cannot be assigned"
      }
    ]
  }
}
```

#### GET /runs/{runId}/team

Consulta equipe definida.

**Response 200:**

```json
{
  "run_id": "run-001",
  "team": [
    {
      "professional_id": "P001",
      "professional_name": "Ana Exemplo",
      "role_id": "gerente-projeto",
      "role_name": "Gerente de Projeto",
      "proven_score": 28,
      "approved_by": "admin",
      "approved_on": "2026-10-21T11:00:00Z"
    }
  ],
  "coverage": {
    "gerente-projeto": {
      "slots": 1,
      "filled": 1,
      "remaining": 0
    }
  }
}
```

---

### 6. Exportação (Exports)

#### POST /runs/{runId}/exports

Gera relatório exportável.

**Request Body:**

```json
{
  "format": "pdf",
  "include_evidence": true,
  "include_breakdown": true,
  "language": "pt-BR"
}
```

**Response 200:**

```json
{
  "export_id": "exp-001",
  "run_id": "run-001",
  "format": "pdf",
  "status": "completed",
  "download_url": "/api/v1/exports/exp-001/download",
  "generated_on": "2026-10-21T12:00:00Z",
  "expires_on": "2026-10-28T12:00:00Z"
}
```

#### GET /exports/{exportId}/download

Download do arquivo exportado.

**Response 200:**

```
Content-Type: application/pdf
Content-Disposition: attachment; filename="relatorio-equipe-run-001.pdf"

[binary file]
```

---

### 7. Documentos (Documents) - Futuro

#### POST /documents

Upload de documento.

**Request:** `multipart/form-data`

| Campo | Tipo | Obrigatório | Descrição |
|-------|------|-------------|-----------|
| `file` | file | Sim | Arquivo (PDF, DOCX) |
| `type` | string | Sim | Tipo (certificado, diploma, atestado) |
| `professional_id` | string | Sim | ID do profissional |

**Response 201:**

```json
{
  "id": "doc-001",
  "professional_id": "P001",
  "type": "certificado",
  "filename": "certificado-pmp.pdf",
  "hash": "sha256:abc123...",
  "size_bytes": 245678,
  "status": "received",
  "uploaded_on": "2026-10-21T10:00:00Z"
}
```

#### GET /documents/{id}/extraction

Consulta status de extração de IA.

**Response 200:**

```json
{
  "document_id": "doc-001",
  "status": "awaiting_review",
  "extraction": {
    "certifications": [
      {
        "name_original": "PMP",
        "name_normalized": "PMP",
        "expires_at": "2027-12-31",
        "source": {
          "page": 1,
          "quote": "Certificação PMP válida até dezembro de 2027"
        }
      }
    ]
  }
}
```

---

## Códigos de Status HTTP

| Código | Descrição |
|--------|-----------|
| 200 | Sucesso |
| 201 | Criado com sucesso |
| 202 | Aceito (processamento assíncrono) |
| 204 | Sucesso sem conteúdo |
| 400 | Requisição inválida |
| 401 | Não autenticado |
| 403 | Não autorizado |
| 404 | Recurso não encontrado |
| 409 | Conflito (ex: versão já aprovada) |
| 422 | Entidade não processável (validação) |
| 500 | Erro interno do servidor |

---

## Códigos de Erro

| Código | Descrição |
|--------|-----------|
| `PROFESSIONAL_NOT_FOUND` | Profissional não encontrado |
| `OPPORTUNITY_NOT_FOUND` | Oportunidade não encontrada |
| `CRITERIA_VERSION_NOT_FOUND` | Versão de critérios não encontrada |
| `RUN_NOT_FOUND` | Execução não encontrada |
| `INVALID_TEAM` | Equipe inválida |
| `CRITERIA_ALREADY_APPROVED` | Critérios já aprovados não podem ser alterados |
| `DUPLICATE_ASSIGNMENT` | Atribuição duplicada |
| `INELIGIBLE_PROFESSIONAL` | Profissional inelegível |
| `VALIDATION_ERROR` | Erro de validação |

---

## Bibliotecas Recomendadas para Mock de API

### Node.js / TypeScript

#### 1. **JSON Server** ⭐ (Recomendado para PoC)

```bash
npm install -D json-server
```

**Configuração:**

```javascript
// server.js
const jsonServer = require('json-server');
const server = jsonServer.create();
const router = jsonServer.router('./data/db.json');
const middlewares = jsonServer.defaults();

server.use(middlewares);
server.use(jsonServer.bodyParser);

// Custom routes
server.get('/api/v1/runs/:runId/results', (req, res) => {
  const results = require('./data/resultados-esperados.json');
  res.jsonp({
    run_id: req.params.runId,
    results: results.results
  });
});

server.use(router);
server.listen(3001, () => {
  console.log('JSON Server is running on port 3001');
});
```

**Vantagens:**
- Zero configuração inicial
- CRUD automático
- Suporte a relacionamentos
- Filtros, paginação e ordenação nativos
- Perfeito para prototipagem rápida

**package.json:**

```json
{
  "scripts": {
    "mock:api": "json-server --watch data/db.json --port 3001 --routes routes.json"
  }
}
```

#### 2. **MirageJS** ⭐ (Mais completo)

```bash
npm install -D miragejs
```

**Configuração:**

```typescript
// mocks/server.ts
import { createServer, Model, Response } from 'miragejs';

export function makeServer({ environment = 'development' } = {}) {
  return createServer({
    environment,
    
    models: {
      professional: Model,
      opportunity: Model,
      scoringRun: Model,
    },
    
    fixtures: {
      professionals: require('../data/perfis-demonstracao.json').profiles,
      opportunities: [require('../data/criterios-demonstracao.json')],
    },
    
    routes() {
      this.namespace = 'api/v1';
      this.timing = 400; // Simular latência
      
      // Profissionais
      this.get('/professionals', (schema, request) => {
        const { available, page = 1, limit = 20 } = request.queryParams;
        
        let professionals = schema.professionals.all();
        
        if (available !== undefined) {
          professionals = professionals.filter(p => p.available === (available === 'true'));
        }
        
        return {
          data: professionals.slice((page - 1) * limit, page * limit),
          pagination: {
            page: parseInt(page),
            limit: parseInt(limit),
            total: professionals.length
          }
        };
      });
      
      this.get('/professionals/:id');
      this.post('/professionals');
      this.patch('/professionals/:id');
      this.delete('/professionals/:id');
      
      // Oportunidades
      this.get('/opportunities');
      this.get('/opportunities/:id');
      this.post('/opportunities');
      
      // Execução de Pontuação
      this.post('/opportunities/:id/runs', (schema, request) => {
        const body = JSON.parse(request.requestBody);
        return {
          run_id: `run-${Date.now()}`,
          status: 'completed',
          executed_on: new Date().toISOString()
        };
      });
      
      this.get('/runs/:id/results', (schema, request) => {
        const results = require('../data/resultados-esperados.json');
        return {
          run_id: request.params.id,
          results: results.results
        };
      });
      
      // Fallback para rotas não definidas
      this.passthrough();
    },
    
    seeds(server) {
      server.loadFixtures();
    },
  });
}
```

**Integração no app:**

```typescript
// src/index.tsx (React) ou src/main.ts
import { makeServer } from './mocks/server';

if (process.env.NODE_ENV === 'development') {
  makeServer({ environment: 'development' });
}
```

**Vantagens:**
- Roda no navegador (intercepta fetch/XHR)
- ORM embutido com relacionamentos
- Testes fáceis com factories
- Simula latência de rede
- Perfeito para desenvolvimento front-end

#### 3. **MSW (Mock Service Worker)** ⭐⭐ (Mais profissional)

```bash
npm install -D msw
npx msw init ./public --save
```

**Configuração:**

```typescript
// mocks/handlers.ts
import { rest } from 'msw';
import profiles from '../data/perfis-demonstracao.json';
import criteria from '../data/criterios-demonstracao.json';
import results from '../data/resultados-esperados.json';

export const handlers = [
  // Profissionais
  rest.get('/api/v1/professionals', (req, res, ctx) => {
    const available = req.url.searchParams.get('available');
    
    let filteredProfiles = profiles.profiles;
    if (available !== null) {
      filteredProfiles = profiles.profiles.filter(
        p => p.available === (available === 'true')
      );
    }
    
    return res(
      ctx.status(200),
      ctx.json({
        data: filteredProfiles.map(p => ({
          id: p.id,
          display_name: p.display_name,
          available: p.available,
          energy_projects_count: p.energy_projects?.length || 0
        })),
        pagination: { page: 1, limit: 20, total: filteredProfiles.length }
      })
    );
  }),
  
  rest.get('/api/v1/professionals/:id', (req, res, ctx) => {
    const profile = profiles.profiles.find(p => p.id === req.params.id);
    
    if (!profile) {
      return res(
        ctx.status(404),
        ctx.json({ error: { code: 'PROFESSIONAL_NOT_FOUND' } })
      );
    }
    
    return res(ctx.status(200), ctx.json(profile));
  }),
  
  // Oportunidades
  rest.get('/api/v1/opportunities/:id', (req, res, ctx) => {
    return res(ctx.status(200), ctx.json(criteria));
  }),
  
  // Execução
  rest.post('/api/v1/opportunities/:id/runs', async (req, res, ctx) => {
    return res(
      ctx.status(200),
      ctx.json({
        run_id: `run-${Date.now()}`,
        status: 'completed',
        executed_on: new Date().toISOString()
      })
    );
  }),
  
  rest.get('/api/v1/runs/:runId/results', (req, res, ctx) => {
    return res(
      ctx.status(200),
      ctx.json({
        run_id: req.params.runId,
        ...results
      })
    );
  }),
];
```

```typescript
// mocks/browser.ts
import { setupWorker } from 'msw';
import { handlers } from './handlers';

export const worker = setupWorker(...handlers);
```

```typescript
// src/index.tsx
import { worker } from './mocks/browser';

if (process.env.NODE_ENV === 'development') {
  worker.start();
}
```

**Vantagens:**
- Usa Service Workers (não intercepta no Node.js, mas funciona no navegador)
- Mais realista (requisições reais no DevTools)
- Excelente para testes E2E
- Suporte a GraphQL
- Pode ser usado em testes com Jest/Vitest

---

### Python (FastAPI/Flask)

#### 4. **Faker + Factory Boy**

```bash
pip install faker factory-boy
```

```python
# factories.py
import factory
from faker import Faker

fake = Faker('pt_BR')

class ProfileFactory(factory.Factory):
    class Meta:
        model = dict
    
    id = factory.Sequence(lambda n: f'P{n:03d}')
    display_name = factory.Faker('name')
    available = factory.Faker('pybool')
```

---

### Recomendação Final para a PoC

**Para desenvolvimento front-end rápido:**

1. **MSW (Mock Service Worker)** - Melhor opção para desenvolvimento moderno
   - Intercepta requisições no navegador
   - Não altera código da aplicação
   - Funciona com qualquer framework
   - Logs visíveis no DevTools

**Para servidor mock standalone:**

2. **JSON Server** - Mais simples para PoC
   - Zero configuração
   - CRUD automático
   - Ideal para demonstrações

**Arquitetura recomendada:**

```
Frontend (React/Vue/Angular)
    │
    ├─ Development: MSW intercepta fetch/axios
    │                └─ Dados de: data/*.json
    │
    └─ Production: API real
                    └─ Repository escolhe fonte de dados
                        ├─ MockRepository (JSON)
                        ├─ DatabaseRepository (PostgreSQL)
                        └─ ExternalAPIRepository
```

---

## Estrutura de Dados Mock

```
data/
├── perfis-demonstracao.json      # Perfis de profissionais
├── criterios-demonstracao.json    # Critérios e oportunidades
├── resultados-esperados.json      # Resultados de execuções
├── db.json                        # DB para JSON Server
└── routes.json                    # Rotas customizadas JSON Server
```

**db.json (JSON Server):**

```json
{
  "professionals": [...perfis-demonstracao.json],
  "opportunities": [{...criterios-demonstracao.json}],
  "scoringRuns": [{...resultados-esperados.json}],
  "teams": []
}
```

---

## Exemplo de Uso Completo

### Front-end com MSW

```typescript
// src/services/api.ts
import axios from 'axios';

const api = axios.create({
  baseURL: process.env.REACT_APP_API_URL || '/api/v1',
  headers: { 'Content-Type': 'application/json' }
});

export const professionalService = {
  getAll: (filters?: { available?: boolean }) => 
    api.get('/professionals', { params: filters }),
  
  getById: (id: string) => 
    api.get(`/professionals/${id}`),
  
  create: (data: CreateProfessionalDTO) => 
    api.post('/professionals', data),
};

export const scoringService = {
  execute: (opportunityId: string, data: ScoringRunDTO) => 
    api.post(`/opportunities/${opportunityId}/runs`, data),
  
  getResults: (runId: string) => 
    api.get(`/runs/${runId}/results`),
};
```

### Troca de Fonte de Dados (Repository Pattern)

```typescript
// src/config/dataSource.ts
import { MockProfileRepository } from '../repositories/MockProfileRepository';
import { DatabaseProfileRepository } from '../repositories/DatabaseProfileRepository';

const USE_MOCK = process.env.DATA_SOURCE === 'mock';

export const profileRepository = USE_MOCK
  ? new MockProfileRepository()
  : new DatabaseProfileRepository();

// Uso no service
// const profiles = await profileRepository.findAll({ available: true });
```

---

## Próximos Passos

1. Implementar camada de Repository com interfaces
2. Criar MockRepository usando os JSONs existentes
3. Configurar MSW para desenvolvimento front-end
4. Implementar serviços que usam os repositories
5. Quando pronto para produção, implementar DatabaseRepository
6. Trocar fonte de dados via variável de ambiente

---

## Referências

- [MSW - Mock Service Worker](https://mswjs.io/)
- [JSON Server](https://github.com/typicode/json-server)
- [MirageJS](https://miragejs.com/)
- [Repository Pattern](https://martinfowler.com/eaaCatalog/repository.html)
- [Dependency Injection](https://en.wikipedia.org/wiki/Dependency_injection)
