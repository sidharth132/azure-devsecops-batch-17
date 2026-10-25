# Class Notes: Automated Database Migration CI Pipeline with PowerShell 🗄️

**Date:** 20-09-2026  
**Topic:** Database CI/CD, Automated SQL Script Execution, PowerShell Automation, and Idempotency

---

## 📌 1. Architecture: Database Migration Pipeline

Automating database schema updates reliably without manual DBA intervention:

```
 [ Azure Repos / GitHub ]
   └── migrations/
         ├── 001_create_devices_table.sql
         ├── 002_create_telemetry_table.sql
         └── 003_add_alert_column.sql
                 |
                 v (Trigger on PR merge)
 [ Azure Pipeline (Database Migration Stage) ]
   └── PowerShell Task iterates over SQL files in numerical order
   └── Executes psql / sqlcmd against Azure PostgreSQL / SQL Server
   └── Records executed migrations in a schema_version tracking table
```

---

## 💻 2. PowerShell Script for Automated Migration

```powershell
$env:PGPASSWORD = "$(dbPassword)" # Read securely from Variable Group / Key Vault

$sqlFiles = Get-ChildItem -Path "$(Build.SourcesDirectory)/migrations/*.sql" | Sort-Object Name

foreach ($file in $sqlFiles) {
    Write-Host "Executing migration: $($file.Name)..."
    & psql -h "axion-postgres.postgres.database.azure.com" `
           -U "devopsadmin" `
           -d "axiondb" `
           -f $file.FullName
    if ($LASTEXITCODE -ne 0) {
        Write-Error "Migration failed on $($file.Name)! Aborting pipeline."
        exit 1
    }
}
```

---

## 💡 Key Takeaways
1. Database migration scripts must be version-numbered and idempotent.
2. Inject database credentials dynamically from Azure Key Vault; never hardcode passwords.
