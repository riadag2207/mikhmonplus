# MikhMon Agent System - AI Development Guide

## Project Overview
MikhMon Agent System is a PHP-based WiFi voucher sales management system built on MikroTik RouterOS. The system enables multi-level agent/reseller management, payment gateway integration, and WhatsApp automation.

## Core Architecture

### Key Components
- `public/` - Customer-facing landing pages for direct voucher purchases
- `agent/` - Agent/reseller dashboard and management interface
- `agent-admin/` - Administrative control panel 
- `api/` - RESTful endpoints for integrations
- `genieacs/` - ONU device monitoring integration

### Data Flow
1. Customer purchases flow: `public/ → Payment Gateway → Database → MikroTik Router → WhatsApp Notification`
2. Agent workflow: `agent/ → Voucher Generation → Database → MikroTik Router`
3. Admin controls: `agent-admin/ → Multi-level Management → Payment/WhatsApp Config`

## Development Workflow

### Local Setup
```bash
# Docker-based development
docker run --name mikhmonplus -p 8082:80 riadag/mikhmonplus:latest
# Default credentials: mikhmon/1234
```

### Database Management
- Schema updates: Run `fix_all_database_issues.php?key=mikhmon-fix-2024` after changes
- Key database files: `install_database_bulletproof.php`, `install_database_complete.php`
- Always use PDO prepared statements for database queries

### Key Integration Points

#### WhatsApp Integration
- Webhook handlers in `api/whatsapp_webhook.php`
- Commands format: `BELI`, `HARGA`, `GEN`, `SALDO`
- Configure gateway in `settings/whatsapp_settings.php`

#### Payment Gateway
- Supported: Tripay, Xendit, Duitku
- Configuration: `agent-admin/payment_gateway_config.php`
- Callback processing in `api/` directory

## Coding Patterns

### Security Practices
- Database operations: Always use PDO prepared statements
- Input validation: `include/validation.php` utilities
- Session management: Check `check_admin.php` for required patterns

### Error Handling
- Webhook errors logged to `logs/webhook_log.txt`
- System errors in `logs/error_log.txt`
- Use built-in logging functions from `include/functions.php`

### Common Pitfalls
- MikroTik API requires valid RouterOS credentials in settings
- WhatsApp commands are case-sensitive
- Payment gateway callbacks must validate signatures

## Testing
- Test webhook endpoints using `check_webhook_log.php`
- Verify payment flows with gateway test modes
- Database integrity checks via `check_database_completeness.php`

## Need Help?
- Core architecture details in `ANALISIS_LENGKAP.md`
- WhatsApp commands documented in `WHATSAPP_WEBHOOK_COMMANDS.md`
- Check `logs/` directory for troubleshooting