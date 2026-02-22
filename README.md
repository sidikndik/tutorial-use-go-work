# Guide to Using Go Workspaces (`go.work`)

Go Workspaces allow you to work on multiple modules simultaneously without the need to manually edit `go.mod` files to add `replace` directives. This is the best approach for a modular system like **ERP-APP**.

---

## 1. Directory Structure Setup
Ensure all modules you want to link are located under a single parent (root) directory.
Based on your setup:

```text
erp-app/ (Root)
├── base-app/
├── component/
│   ├── dto/
│   └── dictionary/
└── modules/
    └── inventory-api/

```

## 2. Initialize the Workspace
Open your terminal in the Root directory (`erp-app`) and run the command to create the `go.work` file:

```bash
# Initialize the workspace with a primary module
go work init ./base-app
```

## 3. Register Modules to the Workspace
Add all folders containing a `go.mod` file to the workspace so Go can resolve these local dependencies

## 4. The go.work File Structure
After running the commands above, a `go.work` file will be generated automatically. It should look like this:

```go
go 1.24 // Adjust to your installed Go version

use (
	./component/dictionary
	./component/dto
	./component/repository
	./modules/inventory-api
	./modules/purchasing-api
)
```
## 5. Synchronize Dependencies
To ensure all modules in the workspace are synchronized (especially regarding versioning and indirect dependencies), run this command in the Root directory:
```bash
go work sync
```

## 6. Removing a Module from Workspace
If you want to stop using a local version and revert to the official GitLab repository:

```bash
go work edit -dropuse ./folder-path
```

## 7. VS Code Troubleshooting
If you see "red lines" or errors even though your code is correct:
1. Run `go mod tidy` inside the specific module folder that shows errors.
2. Open the Command Palette (`Ctrl + Shift + P`).
3. Select `Go: Restart Language Server` to force VS Code to re-index the workspace.

## 8. Key Benefits
1. No `go.mod` Pollution: You don't have to add `replace` lines that might accidentally be pushed to GitLab.
2. Real-time Development: Changes in `component/dto` are immediately reflected in `inventory-api` without needing `git push` or `go get`.
3. Local-Only Configuration: The `go.work` file should be added to your .`gitignore` because the paths are specific to your local machine.