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

## How Skills Work

Skills in Claude Code are **simple directories with a `SKILL.md` file** - no compilation or packaging required! This makes them:

- **Easy to version control:** Skills are just text files you can commit to git
- **Simple to share:** Copy skill directories to `~/.claude/skills/` for personal use, or `.claude/skills/` in your project for team-wide access
- **Automatically available:** When teammates clone a project with skills, they get them instantly
- **Easy to update:** Edit `SKILL.md`, copy the directory, and you're done

This repository serves as a centralized source for Nimble's custom skills, making it easy to maintain, update, and distribute them across teams.

## Skill Management Workflow

### Three Types of Skills

1. **Official Nimble Skills (this repository)**
   - Team-approved skills for company-wide use
   - Version controlled in this repository
   - Examples: `nimble-user-stories`, `nimble-code-review`

2. **Personal Skills**
   - Individual skills you create for your own use
   - Stored in `~/.claude/skills/` on your machine
   - Not shared with the team

3. **Draft Skills (work-in-progress)**
   - Skills you're developing/testing before proposing
   - Can be worked on locally in this repo (auto-ignored by git)
   - Use prefixes: `*-draft/`, `*-wip/`, `draft-*/`, etc.

### Recommended Workflow

```
┌─────────────────────────────────────────────────────────────┐
│ YOUR MACHINE                                                 │
│                                                              │
│  ~/.claude/skills/                                          │
│  ├── nimble-user-stories/    ← Copied from this repo       │
│  ├── my-personal-skill/      ← Your personal skill         │
│  └── another-personal-skill/ ← Your personal skill         │
│                                                              │
│  ~/workspace/claude-skills/ (this repo)                     │
│  ├── nimble-user-stories/    ← Official team skill         │
│  ├── my-skill-draft/         ← Testing new skill (ignored) │
│  └── README.md                                              │
└─────────────────────────────────────────────────────────────┘
```

**Key principles:**
- ✅ This repository = Official Nimble skills only
- ✅ Personal skills = Keep in `~/.claude/skills/` only
- ✅ Draft skills = Use `-draft`/`-wip` suffix when developing locally
- ✅ When draft is ready = Remove suffix, submit PR for review

### Developing a New Skill

1. **Start with a draft locally**
   ```bash
   cd /path/to/claude-skills
   mkdir my-new-skill-draft
   # Work on SKILL.md
   ```

2. **Test it**
   ```bash
   cp -r my-new-skill-draft ~/.claude/skills/
   # Test in Claude Code
   ```

3. **When ready to share**
   ```bash
   # Remove the -draft suffix
   mv my-new-skill-draft my-new-skill

   # Commit and create PR
   git add my-new-skill/
   git commit -m "Add my-new-skill"
   git push origin feature/my-new-skill
   # Create PR for team review
   ```

4. **After approval**
   - Skill becomes official and available to the whole team
   - Others can copy it to their `~/.claude/skills/`

## Repository Structure

```
claude-skills/
├── README.md                    # This file
├── .gitignore                   # Standard ignores
├── nimble-user-stories/         # Example skill
│   └── SKILL.md                 # Skill definition (editable)
└── [other-skills]/              # Additional skills as needed
    └── SKILL.md
```

**What we commit:**
- ✅ Skill directories with SKILL.md files
- ✅ Any bundled resources (scripts/, references/, assets/)
- ✅ Documentation and configuration files

**What we don't commit:**
- ❌ Temporary files or build artifacts
- ❌ IDE or system files (covered by .gitignore)

## Using Skills from This Repository

### For Personal Use (Individual Installation)

1. **Clone the repository**
   ```bash
   git clone git@github.com:nimblehq/claude-skills.git
   cd claude-skills
   ```

2. **Copy skills to your personal Claude directory**
   ```bash
   # Install a specific skill
   cp -r nimble-user-stories ~/.claude/skills/

   # Or install all skills at once
   cp -r */ ~/.claude/skills/
   ```

3. **Use the skill in Claude Code**

   The skill is now available! Claude will automatically invoke it when relevant based on the skill's description.

### For Team Use (Project-Level Installation)

Skills can be automatically shared with your team by committing them to your project's `.claude/skills/` directory:

1. **Set up project skills directory**
   ```bash
   # In your project root
   mkdir -p .claude/skills
   ```

2. **Add skills from this repository**
   ```bash
   # Copy specific skill
   cp -r /path/to/claude-skills/nimble-user-stories .claude/skills/

   # Commit to your project
   git add .claude/skills/
   git commit -m "Add nimble-user-stories skill"
   git push
   ```

3. **Team members get skills automatically**

   When teammates pull your project, they automatically have access to the skills in `.claude/skills/` - no manual installation needed!

## Updating Your Skills

### Personal Skills

```bash
# Navigate to the skills repository
cd /path/to/claude-skills

# Pull latest changes
git pull

# Re-copy updated skills to your personal directory
cp -r nimble-user-stories ~/.claude/skills/
```

### Project Skills

```bash
# Navigate to the skills repository
cd /path/to/claude-skills

# Pull latest changes
git pull

# Navigate to your project
cd /path/to/your-project

# Update the skill
cp -r /path/to/claude-skills/nimble-user-stories .claude/skills/

# Commit and push
git add .claude/skills/nimble-user-stories/
git commit -m "Update nimble-user-stories skill"
git push
```

## Contributing

### Creating a New Skill

1. **Create the skill directory**
   ```bash
   mkdir [skill-name]
   cd [skill-name]
   ```

2. **Create SKILL.md with required frontmatter**
   ```bash
   cat > SKILL.md << 'EOF'
   ---
   name: skill-name
   description: What it does and when to use it (max 1024 chars)
   ---

   # Skill Name

   ## Instructions
   Your detailed guidance for Claude here.
   EOF
   ```

3. **Add any supporting resources** (optional)
   ```bash
   mkdir scripts references assets  # As needed
   # Add your files to these directories
   ```

4. **Test locally**
   ```bash
   # Copy to your personal skills directory
   cp -r [skill-name] ~/.claude/skills/

   # Test the skill in Claude Code
   # Claude will invoke it automatically when relevant
   ```

5. **Add to repository**
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
   # Re-copy to your personal directory
   cp -r [skill-name] ~/.claude/skills/

   # Test the updated skill in Claude Code
   ```

3. **Commit and push**
   ```bash
   git add [skill-name]/
   git commit -m "Update [skill-name]: [description of changes]"
   git push
   ```

## Best Practices

### Repository Management
- **Keep this repo clean:** Only commit official, team-approved skills
- **Use draft prefixes:** Work on new skills with `-draft`/`-wip` suffix locally
- **Personal skills stay local:** Don't commit personal/experimental skills to this repo
- **Pull before developing:** Always `git pull` before creating new skills

### Version Control
- Keep skill source files clean and readable
- Write descriptive commit messages
- Use pull requests for significant changes
- Tag releases for major skill updates
- Review PRs before merging new skills

### Skill Development
- Test skills thoroughly before pushing (copy to `~/.claude/skills/` to test)
- Document any external dependencies
- Keep SKILL.md concise and focused
- Use references/ for detailed documentation
- Write clear descriptions (helps Claude know when to invoke the skill)

### Team Collaboration
- Pull regularly to get latest updates
- Communicate major changes to the team
- Share feedback on skill improvements
- Maintain consistency across skills
- Use PRs for new skills to get team input

## Available Skills

### nimble-user-stories
**Purpose:** Writing user stories following Nimble's product development standards

**Covers:**
- Feature stories with templates and acceptance criteria
- Bug reports with structured documentation
- Chores for technical work
- Backlog organization (modules, features, labels)

**Documentation:** [Nimble Compass - Backlog Management](https://nimblehq.co/compass/product/backlog-management/)

---

### nimble-create-prd
**Purpose:** Creating Product Requirements Documents (PRDs) following Nimble's standard

**Covers:**
- PRD structure with all required sections (Overview, Context, Solution, Scope, Requirements)
- AI-friendly formatting with requirement IDs and structured tables
- Functional and non-functional requirements
- User flows with Mermaid diagrams
- Edge cases, roles, permissions, and dependencies
- Best practices for clarity and completeness

**When to use:**
- Creating new PRDs for features or products
- Defining requirements for multi-team or complex features
- Planning features that need AI-assisted backlog creation

**Documentation:**
- [PRD Standard](https://nimblehq.co/compass/product/technical-documentation/product-requirements-document/)
- [PRD Example](https://nimblehq.co/compass/product/technical-documentation/product-requirements-document-example/)

---

*Additional skills will be listed here as they are added to the repository.*

## Support

- **Skill usage questions:** Ask Claude while using the skill
- **Skill development:** See [Claude Code Skills documentation](https://docs.claude.com/en/docs/claude-code/skills)
- **Repository issues:** Open an issue or contact the team
- **Nimble standards:** Refer to [Nimble Compass](https://nimblehq.co/compass/)