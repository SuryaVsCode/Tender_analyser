# Tender Intelligence Agent

An n8n-based AI agent that monitors government tenders, reads the documents,
and tells small businesses whether to bid, with clause-level reasons.

## What it does
- Monitors the CPPP portal every 15 minutes (new tenders + corrigendums)
- Detects deadline changes and keeps an audit log
- Extracts eligibility, EMD and deadline from tender PDFs with an LLM
- Compares them with a company profile → BID / NO-BID / REVIEW
- Sends the verdict and a missing-documents checklist on Telegram

## Workflows
- `tender-monitor.json`: scraper, change detection, alerts
- `tender-analyzer.json`: PDF analysis and verdict

## How to run
1. Import both JSON files in n8n (Workflows → Import from file).
2. Create credentials: Telegram bot token and Mistral API key.
3. Create two Data Tables: `tenders` (tender_key, ref, title, closing_ist,
   opening_ist, link, first_seen, last_checked) and `change_log`
   (tender_key, ref, title, change_type, old_value, new_value, source, detected_at).
4. In the monitor workflow, set your Telegram chat ID and select the
   Tender Analyser sub-workflow.
5. Execute the monitor workflow. Upload a PDF to the analyzer form to test.

## Limitations
- Reads the 10 latest tenders per poll (full search needs a captcha, not bypassed)
- Company profile is hardcoded
- PDFs are uploaded manually (detail links are session-bound)
- Built for CPPP; designed to extend to other portals
