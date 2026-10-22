# Rastreabilidade de requisitos

| Requisito | Implementação | Evidência automatizada |
|---|---|---|
| RF01 Registro | FinancialRecordUseCase, /records, RecordForm | test_crud_idempotency_and_optimistic_lock; test_validation_and_localized_errors; forms.test.tsx |
| RF02 Propriedade | farm_settings por mês/contexto, FixedCosts | test_settings_are_monthly_versioned_and_persisted; forms.test.tsx |
| RF03 DRE | FarmService.month_closing, Dashboard | test_month_coe_cot_daily_calendar_reconciliation |
| RF04 Indicadores | WorkingMemory.calculate, Diagnosis | test_exact_healthy_indicators; test_no_zero_denominator_or_unknown_indicator_fabrication |
| RF05 Retroatividade | History, CAS, daily-ledger | test_retroactive_edit_only_changes_affected_month; test_daily_ledger_uses_one_scoped_snapshot |
| RF06 Calendário | CalendarOverview, MonthClosingDTO.daily_movements | test_month_coe_cot_daily_calendar_reconciliation; product.test.tsx |
| RF07 Inferência | AbstractInferenceEngine, KnowledgeBase, explanation_module | test_cf_values_and_priority; limites R05/R06/R07; test_explanation_has_formulas_sources_evidence_and_weights |
| RF08 Relatórios | ReportExportUseCase, CSV e Markdown | test_export_has_real_scope_indicators_diagnostics_and_formula_safety; product.test.tsx |
| RF09 Cenários | contexts, scenario_receipts, Scenarios | test_simulation_copy_mutate_delete_never_changes_real; test_scenario_creation_is_idempotent |
| RF10 Demonstração | seed transacional e reset com backup/confirmação | test_bootstrap_and_seed_isolation; test_demo_reset_is_explicit_and_preserves_real_and_other_scenarios |

Casos adicionais cobrem conversão HALF_UP, dinheiro inválido, alocação em fevereiro bissexto, concorrência com um único vencedor, origens HTTP, migração preservando o original, estados vazios, exportação com falha, conteúdo Markdown não confiável, cancelamento sem falsa mensagem de gravação e respostas atrasadas. Os fluxos E2E de navegador existem em `frontend/tests/pecunia.spec.ts`, mas estão bloqueados na infraestrutura descrita em verification.md.
