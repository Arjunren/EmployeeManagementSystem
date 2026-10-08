# Employee Management System

## Overview

Kinfolk People is a secure employee operations workspace for people records, departments, reporting lines, attendance, and leave management.

## Features

- Responsive people dashboard with directory search, attendance, team distribution, engagement, and leave review
- Administrator, HR, manager, and employee roles with scoped records
- Validated employee onboarding for HR and administrators
- Leave requests with weekday calculation, overlap prevention, and allowance enforcement
- PostgreSQL migration, repeatable development seed, tests, Docker, and CI

## Technology Stack

Next.js 16 App Router, React 19, TypeScript 5, Node.js 24, PostgreSQL 18, Prisma ORM 7, Zod 4, and Vitest 5.

## Requirements

Node.js 24+, npm 11+, and PostgreSQL 16+ or Docker Desktop.

## Installation

```bash
git clone https://github.com/Arjunren/employee-management-system.git
cd employee-management-system
npm install
cp .env.example .env
```

## Environment Variables

Set `DATABASE_URL` for runtime PostgreSQL access and `DIRECT_URL` for direct migration access. `SESSION_TTL_HOURS` is optional and defaults to 12. Never commit real secrets.

## Database Setup

```bash
npm run prisma:generate
npm run db:migrate
npm run db:seed
```

Use `npm run db:deploy` for production migrations.

## Running the Application

Run `npm run dev` and open `http://localhost:3000`, or run `docker compose up --build`.

## Running Tests

Run `npm test` for unit tests or `npm run check` for Prisma generation, strict typing, tests, and a production build.

## Default Development Accounts

Development only. All use password `PeopleDemo!2026`: administrator `admin@people.local`, manager `manager@people.local`, and employee `employee@people.local`.

## API Endpoints

| Method | Endpoint | Access | Purpose |
| --- | --- | --- | --- |
| POST | `/api/auth/login` | Public | Authenticate and create an HTTP-only session |
| POST | `/api/auth/logout` | Signed in | Revoke the active session |
| GET | `/api/employees` | Signed in | HR/admin directory, manager reports, or own profile |
| POST | `/api/employees` | Administrator or HR | Create an employee with a one-time temporary password |
| GET | `/api/leave-requests` | Signed in | HR/admin queue or own requests |
| POST | `/api/leave-requests` | Signed in | Request validated time off |

## Folder Structure

`app/` contains pages and protected Route Handlers; `components/` contains the interactive people dashboard; `lib/` provides authentication, validation, and database setup; `repositories/` contains Prisma queries; `services/` contains leave rules; `prisma/` contains schema, migration, and seed; `tests/` contains Vitest suites; and `types/` defines repository contracts.

## Security Notes

Passwords use salted scrypt hashes and constant-time comparison. Raw session tokens stay in HTTP-only, same-site cookies and only SHA-256 digests are stored. Every endpoint authorizes on the server: only administrators/HR can create employees, managers see direct reports, and employees see their own records. Zod bounds inputs, the database enforces identity uniqueness, and leave service rules prevent overlaps and excess allowance. Temporary passwords should be delivered through a protected channel and forced to rotate in production. Add rate limiting, CSRF origin checks, audit trails, MFA/SSO, field-level privacy rules, and managed secrets before internet-facing use.

## Remaining Limitations

The dashboard uses representative preview data. Payroll, benefits, document storage, performance reviews, multi-stage leave approvals, public holidays, and SSO provisioning are outside this starter scope.
