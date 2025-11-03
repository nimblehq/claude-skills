# Nimble Claude Skills

A centralized repository for managing and sharing Claude skills across the Nimble team.

## About This Repository

This repository contains source code for custom Claude skills that extend Claude's capabilities with Nimble-specific knowledge, workflows, and best practices. Skills are version-controlled here for easy collaboration, backup, and team-wide distribution.

## What Are Skills?

Skills are modular packages that give Claude specialized knowledge about:
- Company processes and standards
- Domain-specific workflows
- Tool integrations and templates
- Best practices and guidelines

Think of them as "onboarding guides" that transform Claude into a specialized agent for specific tasks.

## Repository Structure

```
nimble-skills/
├── README.md                    # This file
├── .gitignore                   # Excludes generated .skill files
├── nimble-user-stories/        # Example skill
│   └── SKILL.md                # Skill source (editable)
└── [other-skills]/             # Additional skills as needed
    └── SKILL.md
```

**What we commit:**
- ✅ Skill source directories and SKILL.md files
- ✅ Any bundled resources (scripts/, references/, assets/)
- ✅ Documentation and configuration files

**What we don't commit:**
- ❌ Packaged `.skill` files (generated on-demand)
- ❌ Temporary or build artifacts

## Using Skills from This Repository

### 1. Clone the Repository

```bash
git clone git@github.com:nimblehq/claude-skills.git
cd claude-skills
```

### 2. Package the Skill

```bash
# Package a specific skill
python3 /mnt/skills/public/skill-creator/scripts/package_skill.py [skill-name]

# Example:
python3 /mnt/skills/public/skill-creator/scripts/package_skill.py nimble-user-stories
```

This creates a `.skill` file you can install.

### 3. Install in Claude

**Claude Web/Desktop:**
1. Open Claude
2. Go to Settings → Skills
3. Upload the `.skill` file

**Claude Code CLI:**
```bash
claude code skill install [skill-name].skill
```

## Updating Your Skills

When skills are updated in this repository:

```bash
# Pull latest changes
git pull

# Re-package the updated skill
python3 /mnt/skills/public/skill-creator/scripts/package_skill.py [skill-name]

# Reinstall
claude code skill install [skill-name].skill
```

## Contributing

### Creating a New Skill

1. **Initialize the skill structure**
   ```bash
   python3 /mnt/skills/public/skill-creator/scripts/init_skill.py [skill-name]
   ```

2. **Edit the skill content**
   - Update `SKILL.md` with your instructions
   - Add any resources to `scripts/`, `references/`, or `assets/`

3. **Test locally**
   ```bash
   python3 /mnt/skills/public/skill-creator/scripts/package_skill.py [skill-name]
   claude code skill install [skill-name].skill
   # Test the skill in Claude
   ```

4. **Add to repository**
   ```bash
   git add [skill-name]/
   git commit -m "Add [skill-name] skill"
   git push
   ```

### Updating an Existing Skill

1. **Edit the skill**
   ```bash
   vim [skill-name]/SKILL.md
   ```

2. **Test your changes**
   ```bash
   python3 /mnt/skills/public/skill-creator/scripts/package_skill.py [skill-name]
   claude code skill install [skill-name].skill
   ```

3. **Commit and push**
   ```bash
   git add [skill-name]/
   git commit -m "Update [skill-name]: [description of changes]"
   git push
   ```

## Best Practices

### Version Control
- Keep skill source files clean and readable
- Write descriptive commit messages
- Use pull requests for significant changes
- Tag releases for major skill updates

### Skill Development
- Test skills thoroughly before pushing
- Document any external dependencies
- Keep SKILL.md concise and focused
- Use references/ for detailed documentation

### Team Collaboration
- Pull regularly to get latest updates
- Communicate major changes to the team
- Share feedback on skill improvements
- Maintain consistency across skills

## Available Skills

### nimble-user-stories
**Purpose:** Writing user stories following Nimble's product development standards

**Covers:**
- Feature stories with templates and acceptance criteria
- Bug reports with structured documentation  
- Chores for technical work
- Backlog organization (modules, features, labels)

**Documentation:** [Nimble Compass](https://nimblehq.co/compass/product/backlog-management/)

*Additional skills will be listed here as they are added to the repository.*

## Support

- **Skill usage questions:** Ask Claude while using the skill
- **Skill development:** See [Claude's skill-creator documentation](https://docs.claude.com)
- **Repository issues:** Open an issue or contact the team
- **Nimble standards:** Refer to [Nimble Compass](https://nimblehq.co/compass/)