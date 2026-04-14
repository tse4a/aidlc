# Shell Script Security Hardening

Comprehensive guide to writing secure shell scripts.

## Core Principles

1. **Fail fast** — Stop on first error
2. **Quote everything** — Prevent word splitting and glob expansion
3. **Validate inputs** — Never trust external data
4. **Clean up** — Remove temp files on exit

---

## Strict Mode

Always start scripts with:

```bash
#!/usr/bin/env bash
set -euo pipefail
IFS=$'\n\t'
```

| Flag | Effect |
|------|--------|
| `-e` | Exit on any error |
| `-u` | Error on undefined variables |
| `-o pipefail` | Catch errors in pipes |
| `IFS=$'\n\t'` | Safe word splitting |

---

## Variable Quoting

```bash
# ❌ BAD: Word splitting, glob expansion
echo $filename
rm $temp_file
cd $directory

# ✅ GOOD: Always quote
echo "$filename"
rm "$temp_file"
cd "$directory"

# ✅ GOOD: Arrays
files=("file 1.txt" "file 2.txt")
for f in "${files[@]}"; do
    process "$f"
done
```

### Command Substitution

```bash
# ❌ BAD: Unquoted
result=$(command)
echo $result

# ✅ GOOD: Quoted
result="$(command)"
echo "$result"
```

---

## Input Validation

### Validate Arguments

```bash
validate_filename() {
    local file="$1"
    
    # Check not empty
    if [[ -z "$file" ]]; then
        echo "Error: Filename required" >&2
        return 1
    fi
    
    # Check no path traversal
    if [[ "$file" == *".."* ]]; then
        echo "Error: Path traversal not allowed" >&2
        return 1
    fi
    
    # Check allowed characters only
    if ! [[ "$file" =~ ^[a-zA-Z0-9._-]+$ ]]; then
        echo "Error: Invalid characters in filename" >&2
        return 1
    fi
}
```

### Validate Numeric Input

```bash
validate_port() {
    local port="$1"
    
    if ! [[ "$port" =~ ^[0-9]+$ ]]; then
        echo "Error: Port must be numeric" >&2
        return 1
    fi
    
    if (( port < 1 || port > 65535 )); then
        echo "Error: Port out of range" >&2
        return 1
    fi
}
```

---

## Secure Temp Files

```bash
# ❌ BAD: Predictable, race condition
temp_file="/tmp/myapp.tmp"

# ✅ GOOD: Secure temp file
temp_file="$(mktemp)"

# ✅ GOOD: Secure temp directory
temp_dir="$(mktemp -d)"

# Always clean up
cleanup() {
    rm -rf "$temp_file" "$temp_dir"
}
trap cleanup EXIT
```

---

## Trap Handlers

```bash
#!/usr/bin/env bash
set -euo pipefail

# Global state
temp_files=()

cleanup() {
    local exit_code=$?
    
    # Clean up temp files
    for f in "${temp_files[@]:-}"; do
        rm -f "$f" 2>/dev/null || true
    done
    
    exit "$exit_code"
}

# Register cleanup for all exit paths
trap cleanup EXIT INT TERM

# Create temp file and track it
temp="$(mktemp)"
temp_files+=("$temp")
```

---

## Avoiding Common Vulnerabilities

### Command Injection

```bash
# ❌ VULNERABLE: User input in command
user_input="$1"
eval "echo $user_input"
bash -c "process $user_input"

# ✅ SAFE: Use arrays, avoid eval
user_input="$1"
echo "$user_input"
process "$user_input"
```

### Path Traversal

```bash
# ❌ VULNERABLE: Direct path use
file="$1"
cat "/var/data/$file"

# ✅ SAFE: Validate and resolve
file="$1"
if [[ "$file" == *".."* ]] || [[ "$file" == /* ]]; then
    echo "Invalid path" >&2
    exit 1
fi
# Optionally: resolve and check prefix
resolved="$(realpath -m "/var/data/$file")"
if [[ "$resolved" != /var/data/* ]]; then
    echo "Path escapes allowed directory" >&2
    exit 1
fi
cat "$resolved"
```

---

## Safe Conditionals

```bash
# ❌ BAD: Single brackets, unquoted
if [ $var = "value" ]; then

# ✅ GOOD: Double brackets, quoted
if [[ "$var" = "value" ]]; then

# ✅ GOOD: Numeric comparison
if (( count > 10 )); then

# ✅ GOOD: Default values
if [[ "${var:-}" = "value" ]]; then
```

---

## Logging Without Secrets

```bash
# ❌ BAD: Logging secrets
echo "Connecting with password: $PASSWORD"
curl -u "user:$API_KEY" https://api.example.com

# ✅ GOOD: Redact secrets
echo "Connecting with password: [REDACTED]"
echo "API key: ${API_KEY:0:4}****"

# ✅ GOOD: Use env file, don't log
set +x  # Disable trace before sensitive commands
curl -u "user:$API_KEY" https://api.example.com
set -x  # Re-enable if needed
```

---

## Complete Example

```bash
#!/usr/bin/env bash
#
# Secure script template
#
set -euo pipefail
IFS=$'\n\t'

# Configuration
readonly SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
readonly SCRIPT_NAME="$(basename "$0")"

# State
temp_files=()

# Cleanup handler
cleanup() {
    local exit_code=$?
    for f in "${temp_files[@]:-}"; do
        rm -f "$f" 2>/dev/null || true
    done
    exit "$exit_code"
}
trap cleanup EXIT INT TERM

# Logging
log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $*" >&2
}

error() {
    log "ERROR: $*"
    exit 1
}

# Input validation
validate_input() {
    local input="$1"
    
    if [[ -z "$input" ]]; then
        error "Input required"
    fi
    
    if ! [[ "$input" =~ ^[a-zA-Z0-9_-]+$ ]]; then
        error "Invalid input format"
    fi
}

# Main
main() {
    if [[ $# -lt 1 ]]; then
        error "Usage: $SCRIPT_NAME <input>"
    fi
    
    local input="$1"
    validate_input "$input"
    
    # Create secure temp file
    local temp
    temp="$(mktemp)"
    temp_files+=("$temp")
    
    # Process
    log "Processing: $input"
    echo "$input" > "$temp"
    
    log "Done"
}

main "$@"
```

---

## Checklist

Before committing any shell script:

- [ ] Strict mode: `set -euo pipefail`
- [ ] All variables quoted: `"$VAR"`
- [ ] Inputs validated before use
- [ ] Temp files created with `mktemp`
- [ ] Cleanup trap registered
- [ ] No `eval` or `bash -c` with user input
- [ ] No secrets in logs
- [ ] Double brackets for conditionals: `[[ ]]`
