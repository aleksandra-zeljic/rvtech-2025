# mi20154 - EV Punjači (LocalStack + Serverless)

Ovo je početni kostur projekta koji kombinuje kod iz prethodnih foldera:
- Lambda za sync OCM podataka u DynamoDB (`syncOCMdata.js`)
- Lambda za pretragu punjača po gradu (`getChargersByTown.js`)
- Demo Lambda za `/chargers` (`getChargers.js`)
- S3 statički frontend sa Leaflet mapom (`web/index.html`)
- LocalStack + Serverless infrastruktura (`docker-compose.yml`, `serverless.yml`)

## Preduslovi
- Docker i Docker Compose
- Node.js i npm
- `awslocal` (dolazi uz LocalStack CLI)

## Pokretanje (lokalno)

### 1) Pokreni LocalStack
```bash
docker-compose up -d
```

### 2) Deploy serverless resursa
```bash
npm install
npx serverless deploy
```

### 3) Preuzmi API Gateway ID
```bash
awslocal apigateway get-rest-apis --region us-east-1 --query 'items[0].id' --output text
```

### 4) Ažuriraj `API_ID` u frontendu
U `web/index.html` postavi vrednost za `API_ID` (iz prethodnog koraka).

### 5) Deploy frontend na S3
```bash
npm run deploy-frontend-fixed-bucket
```

### 6) Pristupi aplikaciji
- S3 website:
  `http://punjaci-website.s3-website.localhost.localstack.cloud:4566`
- API primer:
  `http://localhost:4566/restapis/{API_ID}/dev/_user_request_/chargers/Belgrade`

## Napomene
- `API_ID` se menja nakon `docker-compose down -v`, pa je potrebno ponovo ažurirati frontend.
- CORS je podešen na LocalStack S3 website domen.

## Primeri iz ranijih vežbi
U `examples/` su kopirani materijali iz prethodnih foldera:
- `examples/docker-node` (Docker + Node "Hello World")
- `examples/localstack-basic` (LocalStack + Lambda template primeri)
