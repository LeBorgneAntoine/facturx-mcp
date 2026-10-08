# Factur-X by Orvel for n8n

Import [`facturx-invoice.json`](facturx-invoice.json) into n8n using **Import from File**. It uses built-in nodes and is inactive by default.

## First run

1. Get API access from [Factur-X by Orvel](https://facturx.orvel.dev/docs/pricing/). This template is free; generating customer documents uses your account quota. Confirm the current offer before running. Do not use the historical free-plan instructions.
2. Open **Generate invoice once** and select/create an n8n **Header Auth** credential. Header name: `Authorization`. Value: `Bearer YOUR_API_KEY`. Store the real value only in n8n's credential manager, never in a Set node, workflow JSON or screenshot.
3. Execute the included synthetic sample. It is the same invoice shape used by the [API quickstart](https://facturx.orvel.dev/docs/quickstart/). Sample party/bank identifiers are examples, not details for actual billing.
4. Open **Output invoice PDF** → **Binary** → **Download**. Inspect **Output validation report** for findings, including warnings.

One execution makes one generation request for the sample, returning both PDF and validation metadata. No separate paid validation request is made. Errors, incomplete checks, noncanonical base64 and non-PDF payloads stop downstream output. Warnings do not automatically block output.

The workflow requests the `en16931` profile and checks XSD, base schematron and French fr-ctc rules. These checks do not independently certify PDF/A or establish that an invoice has been transmitted/accepted by a Plateforme Agréée or Peppol provider.

## Use your own invoice data

Replace **Edit sample invoice** with your source of structured data, preserving the `invoice` object shown in the sample. Review the [API schema](https://facturx.orvel.dev/api) for supported fields. The workflow does not use an LLM to invent amounts, tax treatment or buyer identifiers. Each input item causes one API request. For production batches, add your own durable duplicate-submission protection and invoice-number allocation before calling the API.

Connect file storage or delivery after **Output invoice PDF**, keeping **Require complete validation** in the path. That node checks the complete response before exposing output. If a later item fails, earlier API calls may already have consumed quota even though the validation node emits no partial output.

## Failure handling and data

- Automatic retries and HTTP redirects are disabled. The request timeout is 60 seconds and the workflow timeout is 120 seconds. A timeout does not prove the server did no work; check account usage before rerunning.
- HTTP 401: check access credentials. HTTP 402: check access/remaining quota. HTTP 400/422: check invoice input. HTTP 429/503: retry manually only after investigating the response/account usage.
- Invoice data is sent to Factur-X by Orvel and processed by your n8n instance. The editor can show request and response data. This template disables saving successful, failed and manual execution data, but deployment settings, binary retention and connected systems also matter. Review your instance's privacy/retention settings before handling customer invoices.
- No trigger is scheduled, no webhook is exposed, and no file/email is sent to a customer by this template.

## Verification

Tested with n8n 2.41.6 against a local synthetic HTTP service: successful binary output, missing required validation, malformed PDF, rejected authentication and HTTP redirect. Each scenario makes one request; failures release no downstream PDF or report. Additional boundary tests cover skipped checks, inconsistent validation, noncanonical base64, wrong output/profile and partial-batch rejection.

These checks verify workflow transport and output handling, not a new live paid purchase or independent PDF/A conformance. No production key or customer data is included. The workflow can be imported directly; no package installation or custom node is needed.
