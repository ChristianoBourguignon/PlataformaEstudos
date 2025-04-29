```markdown
# Plataforma Educacional (Laravel)

> Projeto de estudo em PHP 8.1+ usando Laravel para gerenciar aulas particulares.

---

## 📋 Sumário

1. [Descrição](#descrição)
2. [Funcionalidades](#funcionalidades)
3. [Tecnologias](#tecnologias)
4. [Pré-requisitos](#pré-requisitos)
5. [Instalação](#instalação)
6. [Estrutura](#estrutura)
7. [Banco de Dados & Modelos](#banco-de-dados--modelos)
8. [Rotas & Controllers](#rotas--controllers)
9. [Agendamento & Notificações](#agendamento--notificações)
10. [Pagamentos (Opcional)](#pagamentos-opcional)
11. [Testes](#testes)
12. [Deploy](#deploy)
13. [Roadmap de Estudo](#roadmap-de-estudo)

---

## 📝 Descrição

Aplicação para:

- Professores enviarem exercícios e materiais
- Agendar e gerenciar aulas
- Controlar presença e notas dos alunos
- Notificações automáticas por e‑mail
- (Opcional) Controle de pagamentos

Foco: aprender conceitos de Laravel sem usar outras ferramentas complexas.

---

## 🚀 Funcionalidades

- Autenticação de usuários (professor, aluno)
- CRUD de exercícios e materiais
- Agendamento de aulas
- Registro de presença e notas
- Envio de lembretes por e‑mail
- Painel simples em Blade (HTML, CSS e JS básicos)
- (Opcional) Cobranças via Stripe com Laravel Cashier

---

## 🚀 Funcionalidades

### 🧑‍🏫 Funcionalidades para Professores

- **CRUD de Exercícios e Materiais**: criar, editar, remover e listar exercícios e recursos de estudo.
- **Agendamento de Aulas**: marcar, editar e cancelar aulas com alunos.
- **Gerenciamento de Presença e Notas**: registrar presença e atribuir notas/feedback após cada aula.
- **Notificações**: enviar lembretes automáticos por e-mail sobre aulas e tarefas.
- **Relatórios**: gerar relatórios de presenças, notas e financeiros.
- **Pagamentos (Opcional)**: configurar cobranças únicas ou assinaturas via Stripe.

### 🧑‍🎓 Funcionalidades para Alunos

- **Visualizar Exercícios e Materiais**: acessar, baixar e enviar as tarefas enviadas pelo professor.
- **Agenda de Aulas**: consultar datas e horários das aulas marcadas.
- **Registro de Presença**: confirmar presença nas aulas.
- **Notas e Feedback**: visualizar notas atribuídas e comentários do professor.
- **Notificações**: receber e-mails de lembrete sobre aulas e tarefas.
- **Pagamentos (Opcional)**: efetuar pagamentos e gerenciar assinaturas.

---

## 🛠️ Tecnologias

- PHP 8.1+
- Laravel 10.x
- MySQL
- Blade (templates HTML/CSS/JS)
- Composer (dependências PHP)

[![Laravel](https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)](https://laravel.com)
[![PHP](https://img.shields.io/badge/PHP-777BB4?style=for-the-badge&logo=php&logoColor=white)](https://php.net)
[![MySQL](https://img.shields.io/badge/MySQL-005C84?style=for-the-badge&logo=mysql&logoColor=white)](https://mysql.com)


---

## 🔧 Pré-requisitos

- PHP 8.1+
- Composer
- MySQL instalado
- Servidor web ou `php artisan serve`

---

## 🚀 Instalação

1. **Clone**
   ```bash
   git clone https://github.com/seu-usuario/plataforma-educacional.git
   cd plataforma-educacional
   ```
2. **Dependências**
   ```bash
   composer install
   ```
3. **Configuração**
   - Copie `.env.example` para `.env`
   - Ajuste conexão com MySQL (DB_HOST, DB_DATABASE, DB_USERNAME, DB_PASSWORD)
4. **Chave**
   ```bash
   php artisan key:generate
   ```
5. **Banco de dados**
   ```bash
   php artisan migrate --seed
   ```
6. **Servidor**
   ```bash
   php artisan serve
   ```

---

## 📂 Estrutura

```
app/
 ├─ Models/
 ├─ Http/Controllers/
 ├─ Http/Requests/
 ├─ Notifications/
 └─ Jobs/            # (opcional)

database/
 ├─ migrations/
 └─ seeders/

resources/views/      # Blade templates
public/               # CSS e JS estáticos

routes/
 └─ web.php
```

---

## 💾 Banco de Dados & Modelos

Principais tabelas:

| Tabela      | Campos-chave                         |
|-------------|--------------------------------------|
| users       | id, name, email, password, role      |
| exercises   | id, title, description, user_id      |
| materials   | id, name, path, exercise_id          |
| lessons     | id, professor_id, student_id, date   |
| attendances | id, lesson_id, present(boolean)      |
| grades      | id, lesson_id, grade, feedback       |

Relações básicas entre Models via Eloquent.

---

## 🔗 Rotas & Controllers

Em `routes/web.php`:

```php
Route::middleware('auth')->group(function() {
    Route::resource('exercises', ExerciseController::class);
    Route::resource('materials', MaterialController::class);
    Route::resource('lessons', LessonController::class);
    Route::resource('attendances', AttendanceController::class);
    Route::resource('grades', GradeController::class);
});
```

Controllers para CRUD básico com métodos: index, create, store, show, edit, update, destroy.

---

## ⏰ Agendamento & Notificações

- Use **Scheduler** em `app/Console/Kernel.php`:
  ```php
  protected function schedule(Schedule $schedule)
  {
      // enviar lembretes às 08:00
      $schedule->call(function() {
          // lógica de lembrete
      })->dailyAt('08:00');
  }
  ```
- Para e‑mails, configure `MAIL_MAILER=smtp` no `.env` e use `Mail::to($user)->send(new ReminderMail(...));`.
- Se quiser usar filas, rode: `php artisan queue:work` (opcional).

---

## 💳 Pagamentos (Opcional)

1. Instale Cashier:
   ```bash
   composer require laravel/cashier
   php artisan vendor:publish --tag=billing-migrations
   php artisan migrate
   ```
2. Adicione `Billable` no Model `User`.
3. Configure `STRIPE_KEY` e `STRIPE_SECRET` no `.env`.
4. Exemplos:
   ```php
   $user->charge(5000, ['currency'=>'BRL']);
   $user->newSubscription('main', 'price_id')->create();
   ```

---

## ✅ Testes

- Executar todos:
  ```bash
  php artisan test
  ```
- Escreva testes simples em `tests/Feature` para rotas e controllers.

---

## 🚀 Deploy (Básico)

1. Suba código no servidor
2. Configure `.env` no servidor
3. Rode `composer install`, `php artisan migrate --force`
4. Configure cron:
   ```cron
   * * * * * cd /caminho && php artisan schedule:run
   ```
5. (Opcional) Supervisor para filas: `php artisan queue:work`

---

## 📱 Mobile

Esta plataforma pode ser acessada em dispositivos móveis de duas formas:

### 1. Progressive Web App (PWA)
- Mantém PHP + Laravel no back-end;
- Front-end responsivo com HTML/CSS/JS (Tailwind ou Bootstrap e Alpine.js);
- Adicione `manifest.json` e um `service worker` (ex.: pacote laravel-pwa);
- Funciona offline e permite instalação sem loja de apps.

### 2. App Híbrido / Nativo (Opcional)
- **Ionic + Capacitor**: empacote sua PWA para Google Play/App Store;
- **React Native**: JavaScript/TypeScript para componentes nativos;
- **Flutter**: Dart, performance quase nativa.

> **Recomendação**: para estudos, opte por **PWA** e mantenha o stack simples, focado em PHP + Laravel.

---

## 🛣️ Roadmap de Estudo

1. Configuração e CRUD de `exercises`  
2. Upload de `materials`  
3. Agendamento de `lessons`  
4. Registro de `attendances` e `grades`  
5. Scheduler e e‑mails  
6. (Opcional) Pagamentos com Cashier  
7. Testes  
8. Deploy simples

> Bons estudos!
```

