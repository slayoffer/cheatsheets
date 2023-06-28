### Example

```yaml
resource "local_file" "games" {
  filename     = "/root/favorite-games"
  sensitive_content = "FIFA 21" # wont show in plan logs
}
```

### With vars

```bash
resource "local_file" "state" {
  filename = "/root/${var.local-state}"
  content  = "This configuration uses ${var.local-state} state"
}
```

