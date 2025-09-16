202509161606
- Added an exports map so bundlers resolve the dist entry.

202509161413
- Adjusted Agenda e2e sample to send structured job data so Agenda v5 type checks succeed during execution.
- Verified vitest suite post-fix; attempted pnpm test:e2e but it still only boots the Nest app and needs a manual stop.

202509161353
- Upgraded @nestjs/cli, @nestjs/common, @nestjs/core, @nestjs/platform-express, @nestjs/testing to 11.1.6 to align with the latest NestJS framework release.
- Raised agenda to 5.0.0 to support the newest job runner API surface.
- Refreshed reflect-metadata 0.2.2, rxjs 7.8.2, typescript 5.9.2, @swc/core 1.13.5, unplugin-swc 1.5.7, release-it 19.0.4, rimraf 6.0.1, vitest 3.2.4 to keep tooling compatible with NestJS 11.
- Validated upgrade via pnpm audit, pnpm test, and pnpm run build; no new dependencies added.
