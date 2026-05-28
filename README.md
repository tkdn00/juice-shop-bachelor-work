# 🔐 DevSecOps Pipeline for OWASP Juice Shop

Бакалаврська кваліфікаційна робота — Кидонь Тарас Віталійович, КБ-406
Національний університет «Львівська політехніка», 2026

## Про проєкт

Цей репозиторій є форком [OWASP Juice Shop](https://github.com/juice-shop/juice-shop) 
з інтегрованим DevSecOps pipeline для автоматичного виявлення веб-вразливостей.

## Архітектура pipeline

Pipeline реалізовано у вигляді двох GitHub Actions workflow:

### devsecops-push.yml — повне сканування
Запускається при кожному push у гілку `master`

| Етап | Інструмент | Методологія |
|------|-----------|-------------|
| Статичний аналіз коду | SonarCloud | SAST |
| Аналіз залежностей | Snyk (Node 22-25) | SCA |
| Сканування контейнера | Trivy | Container Security |
| Динамічне тестування | OWASP ZAP | DAST |
| Контроль якості | Quality Gate | — |
| Агрегація звітів | GitHub Issues | — |

### devsecops-pr.yml — інкрементальне сканування
Запускається при створенні Pull Request у гілку `master`

| Етап | Інструмент | Умова |
|------|-----------|-------|
| Статичний аналіз | SonarCloud | Тільки змінені файли |
| Аналіз залежностей | Snyk | Тільки якщо змінились залежності |

## Удосконалення

1. **Quality Gate** — автоматична зупинка pipeline при Critical вразливостях
2. **Агрегація звітів** — зведений звіт у GitHub Issues з усіх інструментів
3. **Інкрементальне сканування** — оптимізація часу для Pull Request (~1.5 хв vs ~15 хв)

## Налаштування

Необхідні GitHub Secrets:
- `SONAR_TOKEN` — токен SonarCloud
- `SNYK_TOKEN` — токен Snyk

## Результати сканування

- **SonarCloud**: 80+ вразливостей (Blocker, Medium)
- **Trivy**: 5 Critical, 32 High у Docker образі
- **Snyk**: аналіз npm залежностей (Node 22-25)
- **OWASP ZAP**: 16 alerts динамічного тестування