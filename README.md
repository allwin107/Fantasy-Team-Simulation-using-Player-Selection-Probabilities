# Fantasy Team Simulation using Player Selection Probabilities

A Python-based solution that generates approximately 20,000 unique fantasy cricket teams, each consisting of 11 players, using weighted selection probabilities to ensure realistic team distributions while respecting cricket team composition constraints.

## Project Overview

This project tackles the complex challenge of simulating fantasy cricket teams where player selections must closely match their expected probability distributions. The algorithm employs advanced optimization techniques to minimize the deviation between expected and actual player selection frequencies across thousands of teams.

### Key Features

- **Large-Scale Simulation**: Generates 20,000 unique fantasy teams efficiently
- **Probability-Based Selection**: Uses weighted random selection based on `perc_selection` values
- **Constraint Adherence**: Ensures each team contains at least one player from each role (WK, Batsman, Bowler, Allrounder)
- **Accuracy Optimization**: Implements iterative refinement to maximize selection accuracy
- **Comprehensive Analysis**: Provides detailed accuracy metrics and error analysis

## Algorithm Performance

The optimized algorithm achieves:
- **Mean Absolute Error**: ~13.93%
- **Players within ±5% error**: 4 out of 22 players
- **Constraint Satisfaction**: 100% of teams contain all required roles
- **Processing Time**: Generates 220,000 player records efficiently

## Project Structure

Fantasy-Team-Simulation-using-Player-Selection-Probabilities/

├── Fantasy_Team_Simulation.ipynb # Main implementation notebook

├── player_data_sample.csv # Sample player data with probabilities

├── README.md # Project documentation

├── teamdf.csv # Generated team output (after execution)

└── accuracy_summary.csv # Accuracy analysis results (after execution)


## Data Schema

### Input Data (`player_data_sample.csv`)
| Column | Description |
|--------|-------------|
| `match_code` | Match identifier |
| `player_code` | Unique player identifier |
| `player_name` | Player name |
| `role` | Player position (WK/Batsman/Bowler/Allrounder) |
| `team` | Team affiliation (A/B) |
| `perc_selection` | Expected selection probability (0-1) |
| `perc_captain` | Captain selection probability |
| `perc_vice_captain` | Vice-captain selection probability |

### Output Data (`teamdf.csv`)
| Column | Description |
|--------|-------------|
| `match_code` | Match identifier |
| `player_code` | Player identifier |
| `player_name` | Player name |
| `role` | Player role |
| `team` | Original team |
| `perc_selection` | Selection probability |
| `team_id` | Generated fantasy team ID (1-20000) |

## Getting Started

### Prerequisites

```bash
pip install pandas numpy matplotlib plotly
```


### Running the Simulation

1. **Clone the repository**:
```bash
git clone https://github.com/allwin107/Fantasy-Team-Simulation-using-Player-Selection-Probabilities.git
cd Fantasy-Team-Simulation-using-Player-Selection-Probabilities
```

2. **Execute the notebook**:
Open `Fantasy_Team_Simulation.ipynb` in Jupyter Notebook or Google Colab and run all cells.

3. **Review outputs**:
- `teamdf.csv`: Complete team assignments (220,000 rows)
- `accuracy_summary.csv`: Player-level accuracy analysis

## Algorithm Details

### Core Components

#### 1. **Constraint-Aware Selection**
- **Phase 1**: Mandatory role selection (1 player per role per team)
- **Phase 2**: Flexible selection (remaining 7 slots filled probabilistically)

#### 2. **Dynamic Weight Adjustment**

**Deficit correction formula**

deficit_ratio = (expected_so_far - current) / expected_so_far

adjustment = max(0.05, 1.0 + deficit_ratio * 3.0)

final_weight = base_weight * adjustment


#### 3. **Iterative Optimization**
- Runs multiple iterations with different random seeds
- Selects the iteration with lowest mean absolute error
- Tracks accuracy improvements across iterations

### Team Composition Rules

Each generated team must contain:
- **Minimum 1 Wicket Keeper (WK)**
- **Minimum 1 Batsman**
- **Minimum 1 Bowler** 
- **Minimum 1 Allrounder**
- **Exactly 11 players total**

## Accuracy Analysis

The algorithm provides comprehensive accuracy metrics:

- **Individual Player Errors**: Percentage deviation from expected selections
- **Role-wise Performance**: Accuracy breakdown by player position
- **Statistical Measures**: Mean absolute error, standard deviation
- **Constraint Validation**: Verification of team composition rules

### Sample Results

| Role | Players | Avg Error | Within ±5% |
|------|---------|-----------|-----------|
| WK | 2 | 68.5% | 0/2 |
| Batsman | 8 | 8.2% | 2/8 |
| Allrounder | 6 | 7.6% | 1/6 |
| Bowler | 6 | 11.7% | 1/6 |

## Technical Challenges Addressed

### 1. **Constraint Paradox**
- **Problem**: Role probability sums don't match minimum requirements
- **Solution**: Dynamic weight adjustment with deficit correction

### 2. **Selection Bias**
- **Problem**: Popular players get over-selected early in generation
- **Solution**: Progressive weight adjustment based on cumulative deficit

### 3. **Optimization Balance**
- **Problem**: Balancing constraint satisfaction with probability accuracy
- **Solution**: Two-phase selection with different optimization strategies

## Usage Examples

### Basic Execution
**Load player data**
players_by_role, df = load_player_data('player_data_sample.csv')

**Generate teams**
teams, selections = generate_teams_optimized(players_by_role, num_teams=20000)

**Create output DataFrame**
team_df = create_team_dataframe(teams)
team_df.to_csv('teamdf.csv', index=False)

**Evaluate accuracy**
accuracy_df = evaluate_team_accuracy(team_df)

### Custom Configuration

**Generate fewer teams with more iterations**
teams, selections = generate_teams_optimized(
players_by_role,
num_teams=10000,
team_size=11,
max_iterations=5
)


## Visualization

The project includes visualization capabilities for:
- Player selection error distributions
- Role-wise accuracy comparisons  
- Team composition validation charts
- Probability vs. actual selection scatter plots

<img width="512" height="341" alt="unnamed" src="https://github.com/user-attachments/assets/e727a71c-fb69-4229-8b07-8de6303bac1a" />


## Contributing

Contributions are welcome! Areas for improvement:
- **Algorithm Enhancement**: Better optimization strategies
- **Visualization**: Enhanced charting and analysis tools
- **Performance**: Optimization for larger datasets
- **Documentation**: Additional examples and use cases

## Author

**allwin107**
- GitHub: [@allwin107](https://github.com/allwin107)
- Repository: [Fantasy-Team-Simulation-using-Player-Selection-Probabilities](https://github.com/allwin107/Fantasy-Team-Simulation-using-Player-Selection-Probabilities)

## Acknowledgments

- Built using Python's scientific computing stack (NumPy, Pandas)
- Visualization powered by Plotly
- Development environment: Google Colab / Jupyter Notebook

---

**Note**: This simulation is designed for educational and analytical purposes in fantasy sports research and algorithm development.
