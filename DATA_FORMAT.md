# Data Format Guide

This guide explains the data format used in [data/data.json](data/data.json) for adding or updating conferences and journals in the AI Journal and Conference Countdown tracker.

## File Structure

The data file is a JSON array containing objects, where each object represents either a conference or a journal.

## Field Definitions

### Required Fields

#### For All Venues (Conferences and Journals)

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `venue` | string | Short name/abbreviation of the venue | `"ICML-26"` |
| `type` | string | Must be either `"conference"` or `"journal"` | `"conference"` |

#### For Conferences Only

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `deadline` | string | Paper submission deadline in format `MM/DD/YYYY HH:MM` | `"01/28/2026 23:59"` |
| `conference_dates` | object | Conference start and end dates | See below |
| `conference_dates.start` | string | Conference start date in format `YYYY/MM/DD` | `"2026/07/12"` |
| `conference_dates.end` | string | Conference end date in format `YYYY/MM/DD` | `"2026/07/18"` |

### Optional Fields

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `venue_long` | string | Full name of the venue | `"International Conference on Machine Learning"` |
| `ranking` | string | Venue ranking (A*, A, B, or custom text) | `"A*"`, `"Top - New"` |
| `area` | string | Research area or topic | `"General AI/ML"`, `"RL"`, `"Vision"` |
| `location` | string | Conference location (for conferences) | `"Seoul"`, `"Amsterdam, Netherlands"` |
| `abstract_deadline` | string | Abstract submission deadline (if different from paper deadline) | `"01/23/2026 11:59"` |
| `note` | string | Additional information or important notes | `"Uses ACL Rolling Review (ARR) system"` |
| `approx` | number | Set to `1` if dates are approximate/tentative | `1` |

## Examples

### Example 1: Complete Conference Entry

```json
{
  "venue": "ICML-26",
  "venue_long": "International Conference on Machine Learning",
  "type": "conference",
  "ranking": "A*",
  "area": "General AI/ML",
  "location": "Seoul",
  "abstract_deadline": "01/23/2026 11:59",
  "deadline": "01/28/2026 23:59",
  "conference_dates": {
    "start": "2026/07/12",
    "end": "2026/07/18"
  }
}
```

### Example 2: Minimal Conference Entry

```json
{
  "venue": "EWRL",
  "venue_long": "European Workshop on Reinforcement Learning",
  "type": "conference",
  "deadline": "06/15/2026 23:59",
  "approx": 1,
  "conference_dates": {
    "start": "2026/09/20",
    "end": "2026/09/23"
  }
}
```

### Example 3: Conference with Abstract Deadline

```json
{
  "venue": "IJCAI-26",
  "venue_long": "International Joint Conference on Artificial Intelligence",
  "type": "conference",
  "ranking": "A*",
  "area": "General AI/ML",
  "location": "Bremen, Germany",
  "abstract_deadline": "01/12/2026 23:59",
  "deadline": "01/19/2026 23:59",
  "conference_dates": {
    "start": "2026/08/15",
    "end": "2026/08/21"
  }
}
```

### Example 4: Journal Entry

```json
{
  "venue": "TMLR",
  "venue_long": "Transactions on Machine Learning Research",
  "type": "journal",
  "ranking": "Recent top tier AI journal",
  "area": "General AI/ML/RL"
}
```

### Example 5: Minimal Journal Entry

```json
{
  "venue": "Nature Machine Intelligence",
  "type": "journal"
}
```

## Date and Time Format Guidelines

### Deadline Format
- **Format**: `MM/DD/YYYY HH:MM`
- **Timezone**: Typically in AOE (Anywhere on Earth) or conference-specific timezone
- **Examples**:
  - `"01/28/2026 23:59"`
  - `"03/05/2026 11:59"`

### Conference Date Format
- **Format**: `YYYY/MM/DD`
- **Examples**:
  - `"2026/07/12"`
  - `"2027/04/24"`

## Common Research Areas

Here are commonly used area values in the dataset:
- `"General AI/ML"` - General artificial intelligence and machine learning
- `"RL"` - Reinforcement learning
- `"NLP"` - Natural language processing
- `"Vision"` - Computer vision
- `"Robotics"` - Robotics and robot learning
- `"Language Models"` - Large language models and foundation models
- `"Graphs"` - Graph learning and graph neural networks
- `"ML Security"` - Machine learning security
- `"Security"` - Cybersecurity
- `"Causal Learning"` - Causal inference and reasoning
- `"RL & Control"` - Reinforcement learning and control theory
- `"Uncertainty in AI"` - Probabilistic methods and uncertainty quantification

## Common Rankings

Typical ranking values used:
- `"A*"` - Top-tier conference (CORE ranking)
- `"A"` - Excellent conference (CORE ranking)
- `"B"` - Good conference (CORE ranking)
- `"Top - New"` - New prestigious conference
- `"Top tier AI journal"` - High-impact journal
- `"Recent top tier AI journal"` - Recently established top journal

## Adding a New Conference

To add a new conference, create a JSON object with the following steps:

1. **Start with required fields**:
   ```json
   {
     "venue": "SHORT-NAME-26",
     "type": "conference",
     "deadline": "MM/DD/YYYY HH:MM",
     "conference_dates": {
       "start": "YYYY/MM/DD",
       "end": "YYYY/MM/DD"
     }
   }
   ```

2. **Add optional but recommended fields**:
   - `venue_long` - Full conference name
   - `ranking` - Conference ranking
   - `area` - Research area
   - `location` - Conference location

3. **Add special fields if applicable**:
   - `abstract_deadline` - If there's a separate abstract deadline
   - `note` - For special submission processes or important information
   - `approx: 1` - If dates are tentative

4. **Insert the object** into the array in chronological order by deadline

## Adding a New Journal

To add a new journal:

1. **Create minimal entry**:
   ```json
   {
     "venue": "JOURNAL-ABBR",
     "type": "journal"
   }
   ```

2. **Add optional fields**:
   - `venue_long` - Full journal name
   - `ranking` - Journal tier/ranking
   - `area` - Primary research area

3. **Insert at the beginning** of the array (journals typically come before conferences)

## Important Notes

1. **JSON Syntax**: Ensure proper JSON syntax with commas between objects, no trailing commas, and matching brackets/braces
2. **Chronological Order**: Conferences are generally ordered by deadline date
3. **Year Suffix**: Include year suffix in venue name for conferences (e.g., `-26` for 2026)
4. **Approximate Dates**: Use `"approx": 1` for tentative dates
5. **Multiple Deadlines**: If a conference has multiple tracks or rounds, consider creating separate entries or using the `note` field
6. **Rolling Deadlines**: For journals or conferences with rolling submissions, use the `note` field to explain the submission process

## Validation Checklist

Before submitting changes, verify:

- [ ] JSON syntax is valid (use a JSON validator)
- [ ] All required fields are present
- [ ] Date formats are correct
- [ ] `type` field is either `"conference"` or `"journal"`
- [ ] Conference entries include `deadline` and `conference_dates`
- [ ] Dates are logical (deadline before conference, start before end)
- [ ] No duplicate venue entries (unless intentional for multiple tracks)
- [ ] Special characters are properly escaped in strings

## Testing Your Changes

After updating [data/data.json](data/data.json):

1. Validate JSON syntax using a JSON validator
2. Open [index.html](index.html) in a browser to verify the display
3. Check that deadlines are correctly calculated
4. Verify sorting and filtering work as expected
