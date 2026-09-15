# Team Onboarding Guide

Welcome to the OpenShift MCP Server documentation! This guide helps your SRE team get started quickly.

## For New Team Members

### Day 1: Get Familiar (30 minutes)

1. **Read the Quick Start** (5 min)
   - [Quick Start Guide](docs/getting-started/quickstart.md)
   
2. **Install Locally** (5 min)
   - [Installation Guide](docs/getting-started/installation.md)
   
3. **Configure Your Client** (5 min)
   - [Cursor Integration](docs/getting-started/cursor-integration.md)
   
4. **Try Your First Query** (15 min)
   - "List all namespaces in my cluster"
   - "Show me any pods in error state"
   - "Give me a quick health check"

### Day 2: Learn Your Role (1 hour)

**Pick Your SRE Workflow:**

- **Cluster Management?** → [Cluster Health Monitoring](docs/workflows/cluster-health.md)
- **Troubleshooting Issues?** → [Troubleshooting & Debugging](docs/workflows/troubleshooting.md)
- **Security & Compliance?** → [Security & Compliance](docs/workflows/security-compliance.md)
- **Performance Work?** → [Performance Tuning](docs/workflows/performance-tuning.md)
- **Backup & Recovery?** → [Disaster Recovery](docs/workflows/disaster-recovery.md)
- **On-Call Rotation?** → [Incident Response](docs/workflows/incident-response.md)
- **Observability Setup?** → [Observability Setup](docs/workflows/observability.md)

Read the workflow document specific to your role.

### Week 1: Practice (Throughout the week)

- Use prompts from your workflow document daily
- Save useful queries to a team document
- Ask questions in team Slack/chat
- Practice on non-prod clusters first

## For Team Leads

### Setup

1. **Clone Repository**
   ```bash
   git clone https://github.com/YamunadeviShanmugam/openshift-mcp-server-demo.git
   ```

2. **Create Team Documentation** (Optional)
   - Add team-specific playbooks
   - Create cluster-specific examples
   - Link to internal tools

3. **Customize MkDocs** (Optional)
   - Edit `mkdocs.yml` for your branding
   - Add team name/logo
   - Customize colors

### Training

1. **Kickoff Meeting**
   - Show the Quick Start (5 min)
   - Demo each SRE workflow (20 min)
   - Answer questions (15 min)

2. **Role-Based Training**
   - On-call: Incident Response workflow
   - Platform: Multi-Cluster Management
   - Security: Security & Compliance workflow
   - Performance: Performance Tuning workflow

3. **Monthly Review**
   - Discuss new SRE workflows
   - Share best practices
   - Identify improvements

## Recommended Learning Path

```
Week 1:
  Day 1: Setup & Quick Start
  Day 2: Cluster Health Monitoring
  Day 3: Troubleshooting & Debugging
  Day 4: One role-specific workflow
  Day 5: Practice & Review

Week 2+:
  Daily: Use in your workflow
  Weekly: Learn new workflow
  Monthly: Share with team
```

## Common Questions

### Q: Do I need kubectl?
**A:** No! The OpenShift MCP Server is a direct API client. Kubectl is not required.

### Q: Is it safe to use in production?
**A:** Yes! Use `--read-only` flag to prevent accidental changes.

### Q: What if I make a mistake?
**A:** Nothing dangerous can happen with `--read-only` mode enabled (default). Without it, follow your normal change procedures.

### Q: Can I use it for multi-cluster?
**A:** Yes! Automatically detects all clusters in your kubeconfig.

### Q: How do I contribute new workflows?
**A:** Create a new `.md` file in `docs/workflows/` and update `mkdocs.yml`.

## Team Resources

### Important Links
- [OpenShift MCP Server GitHub](https://github.com/containers/kubernetes-mcp-server)
- [Model Context Protocol](https://modelcontextprotocol.io)
- [Cursor Documentation](https://cursor.com)

### Internal Links (Customize)
- Team Slack Channel: `#sre-automation`
- Internal Wiki: `wiki.company.com/openshift-mcp`
- Runbook Repository: `github.company.com/runbooks`

## Sharing Useful Queries

Save queries your team finds useful:

```markdown
# [Team Name] Useful Queries

## Quick Health Check
"Give me cluster health: ready nodes, pod status distribution, recent errors"

## Production Incident
"Service is down. Diagnose: pod status, recent deployments, node events, error logs"

## Capacity Planning
"Analyze cluster capacity: utilization trends, growth rate, upgrade recommendations"
```

## Monthly Team Meeting Agenda

```
1. New workflows or features (5 min)
2. Share successful queries (10 min)
3. Common issues encountered (10 min)
4. Best practices review (10 min)
5. Feedback & improvements (5 min)
```

## Success Metrics

Track these to measure team adoption:

- [ ] All team members have installed and configured
- [ ] Each team member uses MCP Server weekly
- [ ] Team has documented 10+ useful queries
- [ ] Documentation site is published
- [ ] New SRE workflows added based on team needs
- [ ] Incident response time decreased
- [ ] Team provides positive feedback

## Support

**Questions?**
- Check the FAQ in the documentation
- Review the relevant workflow guide
- Ask in team chat
- Check GitHub issues

**Found a bug?**
- Report to [OpenShift MCP Server Issues](https://github.com/containers/kubernetes-mcp-server/issues)

**Want to contribute?**
- Submit a pull request to the documentation
- Share your best practices
- Add team-specific workflows

---

**Welcome to the team!** 🚀

Start with the Quick Start guide and reach out if you have any questions!
