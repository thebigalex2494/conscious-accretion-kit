# Contributing to Conscious Accretion Kit

Thank you for your interest in contributing!

## How to Contribute

### Reporting Issues

- Use GitHub Issues for bugs, feature requests, and questions
- Include your OS, Claude Code version, and relevant config

### Pull Requests

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/my-feature`)
3. Make your changes
4. Ensure no personal data is included (run the security check below)
5. Submit a PR with a clear description

### Security Check (Required Before PR)

```bash
# Ensure no personal data leaked
grep -rE "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" . \
  --include="*.md" --include="*.json" --include="*.yaml" --include="*.py" \
  | grep -v "example@example.com" | grep -v "noreply@"
```

This should return zero results.

## What We're Looking For

- **New rule files** -- Development standards for other languages/frameworks
- **Provider implementations** -- New model providers for the engine
- **Platform support** -- Linux/macOS setup scripts, alternative shells
- **Documentation** -- Tutorials, use cases, translations
- **Templates** -- Slash command templates, session examples

## Code of Conduct

Be respectful. Be constructive. Remember that this project exists to help people build systems that work for *them*.

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
