# Repository Map

Saldo is organized across public documentation/demo repositories and private production repositories.

| Repository | Visibility | Role |
| --- | --- | --- |
| [saldo_architecture](https://github.com/unfodl/saldo_architecture) | Public | Architecture diagrams, roadmap, integration notes, and technical documentation. |
| [saldo_web](https://github.com/unfodl/saldo_web) | Public | Web app demo and agent/operations interface prototype. |
| [saldo](https://github.com/unfodl/saldo) | Private | Java core backend for billers, banks, compliance, and settlement rail integrations. |
| [saldowallet_android](https://github.com/unfodl/saldowallet_android) | Private | Native Android non-custodial Saldo wallet in Kotlin. |
| [saldo-stellar](https://github.com/unfodl/saldo-stellar) | Private | TypeScript Stellar wallet and settlement service. |
| [saldo-stellar-ios](https://github.com/unfodl/saldo-stellar-ios) | Private | iOS wallet application and Stellar client implementation. |

## Ownership Boundaries

- `saldo_architecture` is the public source of truth for diagrams, integration scope, and technical roadmap.
- `saldo_web` is the public web demo surface for reviewers and partners.
- `saldo` owns the Java backend and external financial integrations.
- `saldowallet_android` owns the native Android consumer wallet.
- `saldo-stellar` owns TypeScript Stellar transaction submission, monitoring, and wallet infrastructure.
- `saldo-stellar-ios` owns the iOS wallet application.
