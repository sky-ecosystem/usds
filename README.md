# USDS Token and contracts associated

This repository includes 3 smart contracts:

- USDS token
- UsdsJoin
- DaiUsds Converter

### USDS token

This is a standard erc20 implementation with regular `permit` functionality + EIP-1271 smart contract signature validation.

The token uses the ERC-1822 UUPS pattern for upgradeability and the ERC-1967 proxy storage slots standard.
It is important that the `UsdsDeploy` library sequence be used for deploying the token.

#### OZ upgradeability validations

The OZ validations can be run alongside the existing tests:  
`VALIDATE=true forge test --ffi --build-info --extra-output storageLayout`

### UsdsJoin

This is the contract in charge of `mint`ing the erc20 IOUs in exchange of native `vat.dai` balance. It also manages the reverse operation, `burn`ing tokens and releasing `vat.dai`.
A noticeable code difference against `DaiJoin` is this contract doesn't have any permissions system at all.
However, in practice, `UsdsJoin` acts the exact same way as the production `DaiJoin` implementation. This is because there isn't any `wards(address)` set there.

### DaiUsds

It is a permissionless converter between `Dai` and `Usds` (both ways). Using the `public` functions of `UsdsJoin` and `DaiJoin` moves from one token to the other. The exchange rate is 1:1.
It is just a "convenience" contract, users could still get the same outcome executing the separate methods in the `join`s or use any other converter implementation.

## Audits

Audit reports are hosted on the auditors' public portfolio pages:

| Auditor | Report | Date |
| --- | --- | --- |
| Cantina | [Sky: Plasma SkyLink Bridge Deployment Validation](https://cantina.xyz/portfolio/10c453c8-321e-4f11-9466-7906ef39475c) | 2026-04-16 |
| Cantina | [Sky: Avalanche SkyLink Bridge Deployment Validation](https://cantina.xyz/portfolio/8deb51bb-909d-4419-a289-10c105e0269e) | 2026-04-02 |
| ChainSecurity | [USDS Smart Contracts](https://www.chainsecurity.com/security-audit/nst-smart-contracts/) | 2024-09-30 |
| Cantina | [usds](https://cantina.xyz/portfolio/b887d247-8eed-4c25-ae90-98838275a30d) | 2024-09-17 |
| Cantina | [nst upgrades](https://cantina.xyz/portfolio/65fd546e-c95a-4b24-a6ed-86ea564e7113) | 2024-06-14 |
| Cantina | [nst](https://cantina.xyz/portfolio/fd61bf69-e16a-4171-be81-9d3a64625e29) | 2023-09-21 |

ChainSecurity reports are cumulative: each portfolio page hosts the latest version of the report, which covers all previously reviewed versions.

### Other reports

The following reports are not available on an auditor portfolio page and are kept in this repository:

- [Sherlock (20240917-Sherlock_MakerDAO_Endgame_Audit_Report.pdf)](./audit/20240917-Sherlock_MakerDAO_Endgame_Audit_Report.pdf)
