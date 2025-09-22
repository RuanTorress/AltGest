README — Gestão Diário (Flutter PWA)

Aplicativo PWA feito com Flutter para gestão diária de um microempreendedor: tarefas, vendas, caixa, clientes e relatórios rápidos.

Sumário

Sobre

Funcionalidades

Tecnologias

Pré-requisitos

Instalação & execução (desenvolvimento)

Build para produção (PWA / Web)

Deploy sugerido

Persistência de dados

Estrutura do projeto (exemplo)

API / Integração opcional

Testes

Segurança & privacidade

Como contribuir

Licença

Contato

Sobre

Gestão Diário é um PWA (Progressive Web App) construído com Flutter que ajuda microempreendedores a controlar tarefas do dia, registrar vendas, gerenciar caixa e clientes, além de gerar relatórios simples para acompanhamento diário/semana/mês. Funciona offline e sincroniza quando houver conexão (opcional).

Funcionalidades

Cadastro e gerenciamento de tarefas diárias (criar, editar, concluir, priorizar).

Registro rápido de vendas (valor, cliente, forma de pagamento).

Controle de caixa diário (entradas, saídas, saldo).

Cadastro de clientes e histórico de transações.

Relatórios resumidos: vendas por dia/semana/mês, saldo do caixa.

Notificações locais (reminders de tarefas) — onde suportado.

Funciona offline (PWA) e sincroniza com backend quando configurado.

Exportar relatórios em CSV / PDF (opcional).

Tecnologias

Flutter (Dart) — app multiplataforma (Web PWA + Mobile).

Flutter Web PWA (manifest + service worker).

Persistência local: Hive (recomendado para web + mobile) ou Sembast.

(Opcional) Backend: Firebase (Firestore/Auth/Hosting) ou REST API própria.

(Opcional) Firebase Hosting / Netlify / Vercel / GitHub Pages para deploy web.

Pré-requisitos

Flutter SDK (estável) instalado — versão recomendada: >=3.x (verifique com flutter --version).

Dart (vem com Flutter).

Chrome (para rodar Flutter Web em dev).

Node.js + npm (opcional, para ferramentas de deploy).

Firebase CLI (se for usar Firebase Hosting).

Instalação & execução (desenvolvimento)

Clone o repo:

git clone https://github.com/seu-usuario/gestao-diario.git
cd gestao-diario


Instale dependências:

flutter pub get


Rodar em web (modo desenvolvimento):

flutter run -d chrome


Rodar em Android/iOS (se desejar):

flutter run -d <device-id>

Build para produção (PWA / Web)

Gerar build web:

flutter build web


O comando cria a pasta build/web/ com index.html, manifest.json, flutter_service_worker.js, assets, etc.

Testar localmente (serve simples):

cd build/web
python -m http.server 8080   # ou use `npx http-server` / qualquer servidor estático
# abrir http://localhost:8080


Observação: Flutter já gera um manifest.json e um flutter_service_worker.js na pasta build/web. Verifique web/index.html para customizações (meta tags, nome do app, ícones, tema).

Exemplo mínimo do web/manifest.json
{
  "name": "Gestão Diário",
  "short_name": "Gestão",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#0D47A1",
  "icons": [
    {
      "src": "icons/Icon-192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "icons/Icon-512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}

Deploy sugerido

Escolha uma das opções:

Firebase Hosting

firebase login
firebase init hosting
# aponte para build/web como pasta pública
flutter build web
firebase deploy --only hosting


GitHub Pages

Configure action/CI que rode flutter build web e envie build/web para gh-pages.

Netlify / Vercel

Configure para apontar para pasta build/web ou use integração via GitHub Actions.

Persistência de dados

Recomendações:

Hive: leve, sem-SQL, funciona bem em mobile e web. Ideal para dados offline-first (tarefas, vendas, clientes).

Sembast: opção alternativa com API de banco de dados local.

Firebase Firestore: para sincronização em nuvem e multi-device. Usar regras de segurança.

Exemplo de dependências em pubspec.yaml:

dependencies:
  flutter:
    sdk: flutter
  hive: ^2.2.0
  hive_flutter: ^1.1.0
  path_provider: ^2.0.0
  provider: ^6.0.0   # state management simples
  intl: ^0.17.0
  csv: ^5.0.0        # exportar CSV
  pdf: ^3.6.0        # gerar PDF (opcional)

Estrutura do projeto (exemplo)
lib/
├─ main.dart
├─ src/
│  ├─ app.dart
│  ├─ modules/
│  │  ├─ tarefas/
│  │  │  ├─ tarefas_page.dart
│  │  │  ├─ tarefas_controller.dart
│  │  │  └─ tarefa_model.dart
│  │  ├─ vendas/
│  │  └─ caixa/
│  ├─ services/
│  │  ├─ storage_service.dart   # Hive wrapper
│  │  └─ sync_service.dart      # sync com backend (opcional)
│  └─ widgets/
web/
  ├─ manifest.json
  ├─ index.html
  └─ icons/
build/
assets/
pubspec.yaml
README.md

API / Integração opcional

Se desejar sincronização multi-device:

Integre com Firebase Auth + Firestore para armazenar dados do usuário.

Ou exponha uma API REST (Node.js / Django / Laravel) com endpoints:

POST /auth/login

GET /user/:id/tarefas

POST /user/:id/vendas

GET /user/:id/relatorios?from=YYYY-MM-DD&to=YYYY-MM-DD

Lembre-se de tratar conflitos (último a gravar / merge) e oferecer opção de resolver manualmente.

Testes

Unit tests: use flutter test para lógica (controllers, services).

Widget tests: flutter test + flutter_test.

Sugestão CI (GitHub Actions): rodar flutter pub get, flutter test, flutter build web e, se passar, fazer deploy.

Exemplo simples:

name: CI
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: subosito/flutter-action@v2
        with:
          flutter-version: 'stable'
      - run: flutter pub get
      - run: flutter test
      - run: flutter build web --release

Segurança & privacidade

Nunca armazene senhas em texto puro; use Auth providers (Firebase Auth) ou hashing seguro.

Se armazenar dados de clientes, cumpra legislações locais de proteção de dados (por exemplo LGPD no Brasil).

Para sincronização, use HTTPS/TLS e autenticação com tokens curtos/refresh tokens.

Criptografe dados sensíveis no dispositivo se necessário (ex.: hive boxes com cipher).

Como contribuir

Abra uma issue para discutir a mudança.

Faça um fork e crie uma branch: feature/nome-da-feature.

Faça commits pequenos e claros.

Abra um pull request descrevendo a mudança.

Siga o padrão de lint (ex.: flutter analyze) e inclua testes quando possível.
