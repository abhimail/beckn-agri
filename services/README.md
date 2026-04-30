# Generic Service Schema Pack

**Protocol Version:** Beckn 2.0
**Schema Version:** 2.1.0
**Semantic Model:** generalised
**Status:** Initial release

## Overview

The Generic Service schema pack provides a comprehensive set of extension schemas for representing on-demand professional services in the Beckn protocol. It covers the full service transaction lifecycle from discovery through settlement.

## Schemas

This pack includes 7 extension schemas:

1. **ServiceResource** - Resource-level attributes (service units, coverage areas, availability)
2. **ServiceOffer** - Offer-level terms (quantity constraints, booking requirements, cancellation policy)
3. **ServiceContract** - Contract-time metadata (booking channel, client notes)
4. **ServicePerformance** - Execution details (appointment time, location, completion status)
5. **ServiceConsideration** - Pricing and payment terms (fresh payment or entitlement drawdown)
6. **ServiceSettlement** - Payment settlement tracking with on-actuals reconciliation
7. **ServiceEntitlement** - Pre-purchased service capacity issued at procurement commit; drawn down by consuming engagements

## Use Cases

This schema pack is designed for:

- Teleconsultation services (doctors, therapists, advisors)
- Professional services (plumbing, electrical, cleaning)
- Specialized consulting (technical, legal, accounting)
- Healthcare services (appointments, lab tests)
- Education services (tutoring, training)
- Any on-demand service market where providers are professionals

## Design Principles

- **Separation of Concerns**: Each schema captures a distinct aspect of the service transaction
- **Flexibility**: Support for multiple service units, payment methods, and booking channels
- **Completeness**: Cover the full transaction lifecycle from discovery to settlement
- **Genericity**: Applicable across multiple service verticals without modification

## Transaction Flow

### Standard (pay-per-engagement) flow
1. **Discovery** (on_discover): ServiceResource and ServiceOffer in catalogs
2. **Confirmation** (on_confirm): ServiceContract, ServicePerformance, ServiceConsideration (with paymentAuthorisation)
3. **Status Updates** (on_status): ServicePerformance updates, ServiceSettlement tracking

### Procurement (entitlement) flow
1. **Procurement Discovery** (on_discover): ServiceResource and ServiceOffer (bulk/institutional pricing)
2. **Procurement Confirmation** (on_confirm): ServiceContract, ServiceConsideration (milestone payments), ServiceEntitlement issued
3. **Consuming Engagement** (on_confirm per patient/session): ServiceContract, ServicePerformance, ServiceConsideration (with entitlementRef drawing down ServiceEntitlement capacity)
4. **Reconciliation** (on_status / update): ServiceSettlement with on-actuals adjustment; entitlement capacity updated via on_update

## Getting Started

Each schema directory contains:
- `attributes.yaml` - OpenAPI 3.1.1 specification
- `context.jsonld` - JSON-LD context with Beckn imports
- `vocab.jsonld` - Vocabulary definitions for enums
- `profile.json` - Schema metadata and configuration
- `renderer.json` - UI rendering templates (HTML, card, detail views)
- `README.md` - Schema-specific documentation
- `examples/` - Example JSON payloads

## Testing

Run the included test suite to validate the schemas:

```bash
cd tests
python run_tests.py
```

The test suite validates:
- OpenAPI 3.1.1 compliance
- JSON-LD correctness
- Property coverage
- Example payload structure

## Namespace Prefixes

- `sr:` - ServiceResource namespace
- `sof:` - ServiceOffer namespace
- `sc:` - ServiceContract namespace
- `spm:` - ServicePerformance namespace
- `scn:` - ServiceConsideration namespace
- `ssl:` - ServiceSettlement namespace
- `se:` - ServiceEntitlement namespace

## Next Steps

- Review individual schema READMEs for detailed specifications
- Examine examples for typical payload structures
- Run tests to ensure compliance
- Consider domain-specific extensions if needed
