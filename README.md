<p align="center">
  <img src="assets/facturx-blue-f.png" width="128" alt="Factur-X by Orvel">
</p>

<h1 align="center">Factur-X MCP server</h1>

<p align="center">
  Generate, validate and read <b>Factur-X / EN 16931</b> e-invoices over MCP or REST.<br>
  Hosted by Orvel. Factur-X PDF/A-3 · CII · UBL 2.1 · French <code>fr-ctc</code> rules.
</p>

<p align="center">
  <a href="https://facturx.orvel.dev/docs/mcp/">Demo and MCP setup</a> ·
  <a href="https://facturx.orvel.dev/docs/pricing/">Evaluation and plans</a> ·
  <a href="https://facturx.orvel.dev/docs/conformity/">Conformity reports</a> ·
  <a href="https://leborgneantoine.github.io/orvel-status/">Status</a>
</p>

**Endpoint:** `https://facturx.orvel.dev/mcp` (Streamable HTTP)  
**Registry identifier:** `io.github.LeBorgneAntoine/facturx`

For developers integrating invoice generation, validation or extraction into an ERP, SaaS or agent workflow. The service produces and checks files; it is not a Plateforme Agréée or a Peppol access point and does not deliver invoices to recipients.

## Try before paying

Fixed synthetic samples and helper tools work without an API key. Try the [valid sample report](https://facturx.orvel.dev/v1/demo?sample=valid) and [invalid sample report](https://facturx.orvel.dev/v1/demo?sample=invalid), or connect an MCP client and ask:

> Show the invalid invoice demo and explain its findings. Do not upload or process my documents.

The demo accepts only its bundled samples. It is not free processing of your own invoices.

**Your own documents require a paid key:** EUR 3 for 25 operations over 30 days, with no subscription, or monthly plans from EUR 29. Check the [current pricing and terms](https://facturx.orvel.dev/docs/pricing/) before purchase. New free keys are not offered; existing legacy access follows its existing quota.

## Connect

For Cursor, a no-key connection in `.cursor/mcp.json` can inspect the service and run the fixed demo:

```json
{
  "mcpServers": {
    "facturx": {
      "url": "https://facturx.orvel.dev/mcp"
    }
  }
}
```

For paid document processing, add `"headers": { "Authorization": "Bearer YOUR_KEY" }` to the server configuration using your own key. Keep credentials out of version control. The same key and quota cover REST and MCP.

For OAuth setup, Claude, VS Code and other clients, use the maintained [MCP connection guide](https://facturx.orvel.dev/docs/mcp/).

## Tools

| Tool | Purpose |
| --- | --- |
| `view_invoice_demo` | Inspect fixed synthetic valid/invalid samples without uploading a document |
| `get_invoice_example` | Get structured invoice or credit-note examples |
| `explain_finding` | Explain a validation rule |
| `check_party` | Check one invoice party |
| `check_invoice_parties` | Check seller and buyer together |
| `generate_invoice` | Generate Factur-X PDF/A-3, CII or UBL from invoice JSON |
| `embed_xml` | Embed CII XML into an existing PDF |
| `validate_invoice` | Check your document and return structured findings |
| `extract_invoice` | Extract invoice fields from PDF/XML |
| `draft_credit_note` | Draft a full or partial credit note |

Each tool describes its inputs and access requirements. Document operations consume quota; a client should not retry a completed operation blindly.

Generation profiles: BASIC WL, EN 16931, EXTENDED and EXTENDED-CTC-FR. UBL generation supports EN 16931 and EXTENDED-CTC-FR. Reading/validation also supports other Factur-X profiles; see the [API reference](https://facturx.orvel.dev/docs/api/).

## Verify the result

Read `valid`, `findings`, `checks_run` and `checks_skipped` in the report. A successful HTTP response alone does not establish which checks ran, independent PDF/A certification or successful delivery to a PA.

Published sample files and independent Mustangproject validation reports are available on the [conformity page](https://facturx.orvel.dev/docs/conformity/), with scope and known warnings.

## REST API

The same engine is available through:

- `POST /v1/invoices/generate`
- `POST /v1/invoices/embed`
- `POST /v1/invoices/validate`
- `POST /v1/invoices/extract`

Generation expects `{"invoice": {...}, "options": {...}}`, not bare invoice JSON. Use `Accept: application/json` to return file content in base64 alongside totals and the validation report in one request.

[OpenAPI](https://facturx.orvel.dev/openapi.json) · [REST reference](https://facturx.orvel.dev/docs/api/)

## n8n workflow

Import the free [n8n invoice workflow](examples/n8n/README.md) to generate a PDF and check its validation report in one API request. The template uses built-in nodes and your own API credentials. API access and quota follow the [current account offers](https://facturx.orvel.dev/docs/pricing/).

## About

Built and operated by [Orvel](https://orvel.dev/). [Privacy](https://facturx.orvel.dev/docs/legal/privacy/) · [Terms](https://facturx.orvel.dev/docs/legal/terms/) · Support: <support@orvel.dev>.

This repository holds the public manifest, brand assets and connection examples of the hosted service. The engine itself is closed source.
