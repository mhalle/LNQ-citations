# LNQ-citations

Citation tree for lymph node quantification in lung cancer staging

## Download

**Latest database:** [citations.db](../../releases/latest/download/citations.db)

## About

This repository maintains a citation tree database that is automatically refreshed weekly.

## Setup

1. Add your Semantic Scholar API key as a repository secret named `S2_API_KEY`
2. Edit `citetree.yaml` to add seed papers
3. Push to trigger the first crawl

## Configuration

Edit `citetree.yaml` to customize:

- **depth**: How many citation hops to follow (default: 2)
- **direction**: `citations` (papers citing these) or `references` (papers these cite)
- **papers**: List of seed paper IDs

## Querying the Database

The `citations.db` SQLite database contains:

- **papers**: Paper metadata (title, abstract, authors, etc.)
- **paper_references**: Citation edges between papers
- **exploration_roots**: Seed papers and crawl settings

Example queries:

```sql
-- Most cited papers in the tree
SELECT title, citation_count
FROM papers
ORDER BY citation_count DESC
LIMIT 10;

-- Papers citing a specific paper
SELECT p.title
FROM papers p
JOIN paper_references r ON p.paper_id = r.citing_id
WHERE r.cited_id = 'PAPER_ID_HERE';
```

## Manual Refresh

Trigger a manual refresh from the Actions tab or run locally:

```bash
uv tool install s2cli
s2cli citetree add --config citetree.yaml --db citations.db
```

---

Built with [s2cli](https://github.com/mhalle/s2cli) and [Semantic Scholar API](https://api.semanticscholar.org/)
