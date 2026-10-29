# AI Pop Optimizer — All in One

Performance-мод для Stellaris 4.5, объединяющий лучшие идеи существующих модов и исправляющий их главную ошибку: **ИИ не слабеет при ограничении попов**.

> Статус: v0.2.0. Все идентификаторы сверены с автогенерированными логами клиента 4.5.1. Не тестировалось в живой игре — после первого запуска проверяйте `error.log`.

## Принцип (4 компонента)

1. **Кэп попов у ИИ** — `apo_triggers.txt`: `count_owned_pop_amount > 20000` (4.x масштабирует попы ×100, это ≈200 попов старой системы). При превышении рост/сборка замораживаются (`apo_pop_growth_freeze`), при снижении — снимаются.
2. **Компенсация экономики ИИ** — `apo_static_modifiers.txt`: тиры +10/20/30% (`pop_workforce_mult` + `country_*_produces_mult`), чтобы ИИ с меньшим числом попов держал ванильную силу.
3. **Упрощение вакансий** — в 4.x сам пересчёт Paradox уже оптимизировал (поп-группы → workforce, один расчёт на планету); мод сокращает вход — общее число попов ИИ.
4. **Уборка мусорных сущностей** — в планах: ограничение ИИ-геномодификации (меньше суб-видов → меньше поп-групп). Ксеносовместимость в 4.0 больше не создаёт гибридов — пункт устарел.

## Структура

```
.metadata/metadata.json        — манифест (для Workshop)
common/scripted_triggers/     — apo_is_ai_country, apo_is_over_pop_cap (кэп = 20000)
common/scripted_effects/       — apo_monthly_planet_check, apo_recalculate_ai_compensation
common/static_modifiers/      — apo_pop_growth_freeze, apo_ai_econ_comp_1..3
common/on_actions/            — on_monthly_pulse / on_yearly_pulse → apo.1 / apo.2
common/defines/               — справочные константы (НЕ видны движку)
events/apo_monthly.txt        — события apo.1, apo.2
description/description.txt   — описание мода
localisation/english|russian/ — имена модификаторов
```

## Конфигурация

- Кэп попов: `common/scripted_triggers/apo_triggers.txt` (`count > 20000`)
- Бонусы компенсации: `common/static_modifiers/apo_static_modifiers.txt`
- Пороги колоний: `common/scripted_effects/apo_effects.txt` (3/10/20)

## Trade-offs (честно)

- Мод для **одиночной игры**.
- Кэп попов = «ферма» для агрессивного игрока; компенсация смягчает, но не устраняет.
- Баланс цифр — гипотеза, требует тестовой партии (~2500 год).

## Дизайн-факты 4.x (dev diary #370, #372)

- Поп-группы (вид × страта × этика × фракция) — единица расчёта; одна группа дороже одного попа старой системы.
- Workforce назначается вакансиям одним расчётом на планету.
- Имена сверены с `4.5.1_game_{triggers,modifiers,effects}.log` (OldEnt).

## TODO / roadmap

- [ ] Тест в живой игре + `error.log`
- [ ] Ограничение ИИ-генномодификации (меньше суб-видов → меньше поп-групп)
- [ ] Фильтр спама хабитатов ИИ
- [ ] Варианты калибровки под уровни сложности (Commodore / Grand Admiral)
