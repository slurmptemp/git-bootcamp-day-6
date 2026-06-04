# LAB — день 6

> Скопируйте в `LAB.md` в корне `git-bootcamp-day-6` и заполните по ходу работы.

## Базовая задача — `01-clean-history`

### Часть 0 — исследование пяти «грязных» коммитов

По одной строке на коммит — что он добавляет в проект:

| Коммит на `feat/rds` | Что добавляет (1 строка) |
|----------------------|--------------------------|
| `начало rds` | `определение переменных db_name, db_username и db_password в variables.tf` |
| `wip` | `значения aws_region, db_name, db_username и db_password в terraform.tfvars` |
| `fix` | `определены ресурсы aws_db_instance и в main.tf` |
| `sg для базы` | `определена группа безопасности aws_security_group_rule в main.tf` |
| `аутпут готов` | `задан вывод db_endpoint в outputs.tf` |

```bash
# команды, которыми смотрели:
# git log --oneline -5
969830c (HEAD -> feat/rds, origin/feat/rds) аутпут готов
8360d5a sg для базы
16157a6 fix
0369908 wip
e3a141b начало rds
➜  aws-infra git:(feat/rds) ✗ 
```

### Часть 1 — `rebase -i`

Какие команды в редакторе использовали для каждого из 5 коммитов и почему:

| Исходное сообщение | Команда (`reword`/`fixup`/`squash`/…) | Зачем |
|--------------------|----------------------------------------|-------|
| начало rds | `reword` | `формируем начальный конвенциональный commit message` |
| wip | `squash` | `объединяем с предыдущим коммитом, включая сообщение` |
| fix | `fixup` | `объединяем с предыдущим коммитом, текущее сообщение выбросить` |
| sg для базы | `reword` | `поменять сообщение коммита` |
| аутпут готов | `fixup` | `объединяем с предыдущим коммитом, текущее сообщение выбросить` |

**Чем `fixup` отличается от `squash` (одно предложение):**

`squash` объединяет коммит с предыдущим делая сообщение N и N-1 коммитов общими, `fixup` выбрасывает сообщение N, берёт от N-1.

```bash
➜  aws-infra git:(feat/rds) ✗ git --no-pager lg   
* 2e0afc5 (HEAD -> feat/rds) feat(rds): add security group rule and output
* 3401164 feat(rds): add RDS MySQL instance with subnet group
| * 969830c (origin/feat/rds) аутпут готов
| * 8360d5a sg для базы
| * 16157a6 fix
| * 0369908 wip
| * e3a141b начало rds
|/  
* e18b12c (origin/main, origin/HEAD, main) feat(outputs): expose public IP and instance ID
* 99ba77f feat(compute): add EC2 instance and key pair
* 9f4b838 feat(network): add VPC, subnets and security groups
* cb013f5 init: scaffold terraform project structure
➜  aws-infra git:(feat/rds) ✗ 
```

### Часть 2 — cherry-pick

- Хеш хотфикса на `main`: `6b5a51e`
- Хеш cherry-pick коммита на `release/1.0`: `89b1ba2`
- Почему хеши разные (2–3 предложения): `cherry-pick вычисляет diff изменений и вносит, новым коммитом, правки на основании diff`

```bash
# git log --oneline release/1.0 -3
89b1ba2 (HEAD -> release/1.0, origin/release/1.0) fix(compute): upgrade instance_type from t2.micro to t3.micro
e18b12c feat(outputs): expose public IP and instance ID
99ba77f feat(compute): add EC2 instance and key pair
```

---