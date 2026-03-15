# Docker Compose Stacks

Collection de stacks Docker Compose pretes a l'emploi pour le developpement local.

Chaque stack est autonome, documentee et suit les bonnes pratiques : pas de secrets en dur, healthchecks, versions d'images fixees.

> Projet realise dans le cadre du [DevOps Bootcamp TechWorld with Nana](https://www.techworld-with-nana.com/) (2025).
> L'implementation, la documentation et les ameliorations sont mon travail personnel.

---

## Stacks disponibles

| Stack | Services | Ports |
|-------|----------|-------|
| [node-mongo](node-mongo/) | Node.js + MongoDB + Mongo Express | `3000`, `27017`, `8081` |
| [java-postgres](java-postgres/) | Java Spring Boot + PostgreSQL | `8080`, `5432` |

## Demarrage rapide

```bash
# 1. Choisir une stack
cd node-mongo/

# 2. Copier le fichier d'environnement et remplir les valeurs
cp .env.example .env

# 3. Lancer la stack
docker-compose up -d

# 4. Verifier que tout tourne
docker-compose ps
```

## Bonnes pratiques appliquees

### Secrets externalises
Les mots de passe ne sont jamais en dur dans les fichiers Compose. On utilise un fichier `.env` (ajoute au `.gitignore`) avec un `.env.example` comme template.

### Healthchecks
Chaque service a un healthcheck Docker pour s'assurer qu'il est reellement operationnel, pas juste "started". Les services dependants utilisent `depends_on` avec `condition: service_healthy`.

### Versions d'images fixees
On utilise `mongo:7.0` au lieu de `mongo` (tag `latest`). Ca evite les surprises quand une nouvelle version majeure sort et casse la compatibilite.

### Networks nommes
Chaque stack a son propre network nomme pour isoler les services et eviter les conflits entre stacks.

## Structure

```
.
├── node-mongo/
│   ├── docker-compose.yaml
│   ├── .env.example
│   └── README.md
├── java-postgres/
│   ├── docker-compose.yaml
│   ├── .env.example
│   └── README.md
├── .github/
│   └── workflows/
│       └── validate.yml       # CI : validation des Docker Compose
├── .gitignore
└── README.md
```
