# 🦫 any2go

**Turn any data format into type-safe Go code. Stop writing boilerplate.**

`any2go` is a CLI tool that generates idiomatic Go structs and client code from sample data (JSON, YAML, CSV, etc.). It's born from the frustration of manually writing and maintaining Go structs for API integrations, data processing, and configuration files.

## ✨ The Problem It Solves

You're integrating with a REST API. You get a JSON response. You then spend 30 minutes:
1. Staring at the JSON
2. Manually writing a Go struct with `json:` tags
3. Writing `Unmarshal` boilerplate
4. Missing nullable fields, guessing types
5. Repeating this for every endpoint

**`any2go` automates this in 3 seconds.**

## 🚀 Quick Start

```bash
# Install
go install github.com/yourname/any2go@latest

# Generate Go structs from a JSON file
any2go json --sample user.json --name User --pkg models

# Generate from a live API endpoint
any2go api --url https://api.example.com/users/1 --name User

# Output to a specific file
any2go json --sample data.yaml --name Config --output configs/types.go
```

## 📋 Features

### Phase 1
- **JSON to Go structs** with proper `json:` tags
- Intelligent type inference (`string` → `time.Time`, `"123"` → `int`)
- Nested struct generation
- Package declaration
- Output to stdout or file

### Phase 2
- **API client generation** (HTTP functions with error handling)
- **YAML support** for config files
- **CSV/TSV support** for data files
- **Struct updates** (add new fields to existing structs)
- **SQL tags** for database models (`db:"column_name"`)

### Phase 3
- **OpenAPI/Swagger integration** (generate entire SDKs)
- **Protocol Buffers support**
- **GraphQL schema conversion**
- **Interactive mode** with TUI

## 📖 Usage Examples

### Basic JSON Conversion
```bash
# From a file
any2go json --sample response.json --name APIResponse

# From stdin
curl -s https://api.example.com/data | any2go json --name Data

# With custom package name
any2go json --sample config.json --name Config --pkg models
```

### Generate API Client (Planned)
```bash
# This will create a ready-to-use API client
any2go scaffold --url https://api.example.com --name ExampleClient

# Outputs:
# - client.go (HTTP client with auth)
# - types.go (all structs)
# - users.go (GetUser, CreateUser functions)
# - posts.go (Post-related functions)
```

## 🛠️ Installation

### From Source
```bash
git clone https://github.com/yourname/any2go.git
cd any2go
go install ./cmd/any2go
```

### Using Go Install
```bash
go install github.com/yourname/any2go@latest
```

## 🧩 How It Works

1. **Parses** input data (JSON, YAML, CSV, etc.)
2. **Analyzes** structure and values
3. **Infers** appropriate Go types:
   - `"2024-01-01T12:00:00Z"` → `time.Time`
   - `"123.45"` → `float64`
   - `null` fields → pointers
   - Arrays → slices
4. **Generates** clean, idiomatic Go code
5. **Optionally** creates HTTP client functions

## 📁 Project Structure
```
any2go/
├── cmd/any2go/
│   └── main.go          # CLI entry point
├── internal/
│   ├── parser/          # Format parsers (JSON, YAML, CSV)
│   ├── generator/       # Go code generation
│   └── infer/           # Type inference engine
├── pkg/
│   └── types/           # Shared types
└── examples/            # Usage examples
```

## 🧪 Example Output

**Input JSON:**
```json
{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com",
  "created_at": "2024-01-01T12:00:00Z",
  "tags": ["user", "admin"],
  "metadata": {
    "active": true,
    "score": 95.5
  }
}
```

**Generated Go:**
```go
package models

import "time"

type User struct {
	ID        int       `json:"id"`
	Name      string    `json:"name"`
	Email     string    `json:"email"`
	CreatedAt time.Time `json:"created_at"`
	Tags      []string  `json:"tags"`
	Metadata  struct {
		Active bool    `json:"active"`
		Score  float64 `json:"score"`
	} `json:"metadata"`
}
```

## 🎯 Why any2go?

| Scenario | Without any2go | With any2go |
|----------|---------------|-------------|
| New API integration | 30+ minutes manual work | 3 seconds |
| API schema changes | Find/replace all fields | Regenerate |
| Multiple endpoints | Copy-paste errors | Consistent generation |
| Team collaboration | Inconsistent structs | Standardized output |

## 📝 Development Roadmap

### Phase 1: Core JSON Support (Week 1)
- [x] Basic JSON parsing
- [x] Struct generation with JSON tags
- [x] Type inference (int, float, string, bool)
- [x] CLI interface
- [ ] Nested object support
- [ ] Array/slice detection

### Phase 2: Enhanced Features (Week 2)
- [ ] YAML support
- [ ] Time type detection (ISO8601, RFC3339)
- [ ] Nullable field detection (pointers)
- [ ] Custom type mappings
- [ ] Output formatting (go fmt compatible)

### Phase 3: API Client Generation (Week 3)
- [ ] HTTP client boilerplate
- [ ] GET/POST/PUT/DELETE templates
- [ ] Error handling patterns
- [ ] Authentication helpers

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests
5. Submit a pull request

## 📄 License

MIT License - see LICENSE file for details.

## 💭 Inspiration

Born from the frustration of writing the same Go boilerplate every time you integrate with a new API, process a new data format, or parse a new config file. `any2go` is the tool you wish existed when you wrote your first Go struct.

---

**Stop writing boilerplate. Start generating.**
