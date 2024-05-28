# Ejemplo de despliegue automático para una aplicación nodejs

Crear el archivo .github/workflows/main.yml dentro del directorio raíz del proyecto con el siguiente contenido:
```yaml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

  workflow_dispatch: # Para poder ejecutar despliegues manuales desde github

jobs:
  build:

    runs-on: ubuntu-latest

    if: github.ref == 'refs/heads/main'

    strategy:
      matrix:
        node-version: [18.x]

    steps:
      - uses: actions/checkout@v2

      - run: npm ci

      - name: rsync deployments
        uses: burnett01/rsync-deployments@5.1
        with:
          switches: -avzr --delete --exclude '/app/assets/img/framesets/*' --exclude '/app/assets/img/bars/*' --exclude '/app/assets/img/groupsets/*' --exclude '/app/assets/img/wheels/*' --exclude '/app/assets/img/tyres/*' --exclude '/app/assets/img/seatpots/*' --exclude '/app/assets/img/saddles/*'
          path: ./*
          remote_path: /var/www/node-app/
          remote_host: ${{ secrets.HOST }}
          remote_user: ${{ secrets.USER }}
          remote_key: "${{ secrets.SSH_PRIVATE_KEY }}"
```