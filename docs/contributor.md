# Contributing

## Development Setup

1. Fork the repository
2. Clone your fork
3. Install dependencies
4. Setup development environment

## Code Standards

### Style Guide

- Use clang-format
- Follow naming conventions
- Document public APIs
- Write unit tests

### Commit Guidelines

```markdown
type(scope): description

[optional body]

[optional footer]
```

Types:

- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation
- `style`: Formatting
- `refactor`: Code restructuring
- `test`: Adding tests
- `chore`: Maintenance

## Pull Request Process

1. Update documentation
2. Add unit tests
3. Update CHANGELOG
4. Submit PR with description

## Development Workflow

1. Create feature branch

```bash
git checkout -b feature/new-feature
```

1. Make changes

```bash
git add .
git commit -m "feat: add new feature"
```

1. Push changes

```bash
git push origin feature/new-feature
```

1. Create Pull Request

## Testing

### Running Tests

```bash
cd build
ctest --output-on-failure
```

### Writing Tests

```cpp
TEST_CASE("Entity creation") {
    Registry registry;
    auto entity = registry.spawn_entity();
    REQUIRE(entity != null);
}
```

## Code Review Process

1. All code changes require review
2. Reviewers should check:
   - Code quality
   - Test coverage
   - Documentation
   - Performance impact

## Additional Guidelines

- Write meaningful commit messages
- Keep PRs focused and small
- Update documentation when needed
- Add tests for new features
- Follow existing code style

## Need Help?

If you need help with:

- Setting up your development environment
- Understanding the codebase
- Making your first contribution

Please:

- Check existing documentation
- Open an issue for questions
- Ask in pull request comments
