# WxPreUpgradeAnalyzer

A webMethods Integration Server package that helps analyze the impact of upgrading to newer versions by identifying deprecated services, changed APIs, missing dependencies, and custom code that may require updates.

## Purpose

Before upgrading webMethods Integration Server, use this tool to:
- Identify custom services using deprecated or removed built-in services
- Find services affected by API changes in newer versions
- Detect missing dependencies and unresolved references
- Analyze adapter connections and custom Java service usage
- Generate Excel reports for upgrade planning and impact assessment

## Installation & Setup

1. **Install the Package**
   - Copy the WxPreUpgradeAnalyzer package to your Integration Server's `packages` directory
   - Restart Integration Server or reload packages
   - The Pre-Upgrade Analyzer will appear under the Solutions menu in the webMethods Admin UI

2. **Configure File Access**
   - Open `WmPublic/config/fileAccessControl.cnf`
   - Add `packages/WxPreUpgradeAnalyzer/pub/reports` to:
     - `allowedReadPaths`
     - `allowedWritePaths`
     - `allowedDeletePaths`
   - Reload the WmPublic package

3. **Access the Tool**
   - Navigate to Solutions → Pre-Upgrade Analyzer in the Admin UI
   - The tool will open in a new browser tab

## Available Reports

The tool generates Excel (`.xlsx`) reports for the following analyses:

### 1. **Deprecated Dependencies Report**
Identifies custom services using deprecated built-in services that may be removed in future versions.
- **Inputs**: Package name filter (regex, optional)
- **Use Case**: Find services that need refactoring before upgrade

### 2. **Changed Dependencies Report**
Lists custom services using built-in services whose APIs have changed in newer versions.
- **Inputs**: Package name filter (regex, optional), Changed Since Version (required)
- **Use Case**: Identify services requiring signature updates after upgrade

### 3. **Missing Dependencies Report**
Detects custom services referencing non-existent services or document types.
- **Inputs**: Dependent package filter (regex, optional), Ignore system packages (yes/no), Referenced asset filter (regex, optional)
- **Use Case**: Find broken references before migration

### 4. **Package Dependencies Report**
Shows all custom services that depend on a specific package.
- **Inputs**: Package name (required), Dependent package filter (regex, optional)
- **Use Case**: Assess impact of removing or upgrading a package

### 5. **Java Services Report**
Lists custom services using custom Java services.
- **Inputs**: Package name filter (regex, optional)
- **Use Case**: Identify Java code dependencies for code review

### 6. **Debug Dependencies Report**
Finds custom services using built-in debug services (should not be in production).
- **Inputs**: Package name filter (regex, optional)
- **Use Case**: Ensure debug code is removed before production deployment

### 7. **Adapter Assets Report**
Catalogs custom adapter connections, services, listeners, and notifications.
- **Inputs**: Adapter type (optional, defaults to all registered adapters)
- **Use Case**: Document adapter usage for upgrade planning

## Using the Tool

1. **Create Reports**
   - Click on any report link in the "Create Reports" section
   - Fill in required inputs (marked with asterisk `*`)
   - Optional inputs are shown in *italics*
   - Use regex syntax for filters (e.g., `My.+` matches packages starting with "My")
   - Click "Create Report" and wait for generation to complete

2. **Download & Manage Reports**
   - Click "Report List" in the "Download & Manage Reports" section
   - View all generated reports with download links
   - Select reports using checkboxes and delete if needed
   - Reports are stored in `packages/WxPreUpgradeAnalyzer/pub/reports/`

## Version Tracking

The tool tracks deprecated and changed services across webMethods versions using configuration files:
- `config/deprecatedServices.json` - Services deprecated in each version
- `config/changedServices.json` - Services with API changes in each version
- `config/removedServices.json` - Services removed in each version
- `config/debugServices.json` - Debug services that should not be in production

## Example Workflow

**Scenario**: Upgrading from webMethods 10.5 to 11.1

1. Run **Deprecated Dependencies Report** to find services using APIs deprecated in 10.5 or earlier
2. Run **Changed Dependencies Report** (Changed Since Version: 10.5) to identify API signature changes
3. Run **Missing Dependencies Report** to catch any broken references
4. Review reports and plan refactoring work
5. Update custom services as needed
6. Re-run reports to verify all issues are resolved

## Technical Details

- **Report Format**: Excel (.xlsx) using Apache POI library
- **Filter Syntax**: Standard Java regex patterns
- **Package Version**: 11.1
- **JVM Requirement**: Java 11+

## Support & License

These tools are provided as-is and without warranty or support. They do not constitute part of the IBM product suite. Users are free to use, fork and modify them, subject to the license agreement. While IBM welcomes contributions, we cannot guarantee to include every contribution in the master project.

## Known Issues

See the repository issues for current bugs and feature requests.
