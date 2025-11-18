# Learning Buddy Data & ML - Data Understanding & Preparation

## 📋 Project Overview

This project focuses on data understanding and preparation for building a **recommendation system** that maps Learning Paths (LPs) to Courses and enriches them with skill/technology information from the Learning Buddy platform.
will be used for developing model and projcets capstone needs. 

## 📊 Dataset Summary

### Primary Data Sources

| Dataset | Rows | Columns | Purpose |
|---------|------|---------|---------|
| **LP + Course Mapping** | 7,916 | 4 | Core mapping between Learning Paths and Courses |
| **Learning Path Answer** | - | 6 | Enrichment data with course metadata (summary, description, technologies) |
| **Output (Merged)** | 7,916+ | 9+ | Final prepared dataset for recommendation system |

### Key Columns

**From LP + Course Dataset:**
- `learning_path_name` - Target learning path (e.g., "AI Engineer", "Back-End Developer JavaScript")
- `course_name` - Associated course name
- `course_level_str` - Course difficulty level (Dasar, Pemula, Menengah, etc.)
- `tutorial_title` - Individual tutorial/lesson within the course

**From Learning Path Answer Dataset (Enrichment):**
- `summary` - Brief course description
- `description` - Detailed course explanation
- `technologies` - Comma-separated technologies/skills (e.g., "AI/Kognitif,Data,Machine Learning")
- `courseMeta` - Course metadata like duration and rating
- `courseInfo` - Student enrollment and module information

## 📈 Data Insights

### Learning Path Distribution

**Top Learning Paths by Course Count:**
```
Back-End Developer JavaScript    1,022
Front-End Web Developer            894
Android Developer                  663
React Developer                    645
Back-End Developer Python          613
AI Engineer                        608
Data Scientist                     589
```

### Course Level Distribution

The dataset contains courses at various difficulty levels:
- **Dasar** (Beginner)
- **Pemula** (Novice)
- **Menengah** (Intermediate)
- **Lanjutan** (Advanced)

### Data Quality

**Missing Values:** ✅ No null values detected in merged dataset
- All core columns are complete
- No gaps in learning_path_name, course_name, course_level_str, tutorial_title

## 🔄 Data Preparation Process

### Step 1: Load Primary Dataset
```python
df_lp_course = pd.read_csv("data/LP and Course Mapping - LP + Course.csv")
# Shape: (7916, 4)
```

### Step 2: Load Enrichment Dataset
```python
df_rs_learning_path_answr = pd.read_csv("data/Resource Data Learning Buddy - Learning Path Answer (1).csv")
```

### Step 3: Merge Datasets
```python
df_merge = df_lp_course.merge(
    df_rs_learning_path_answr[["summary", "description", "technologies", "courseMeta", "courseInfo", "name"]],
    left_on="course_name", 
    right_on="name"
)
```

### Step 4: Clean & Export
```python
df_merge = df_merge.drop(columns=['name'])
df_merge.to_csv("data/output/lp_course_skill_info", index=False)
```

## 📁 Output Dataset

**File:** `data/output/lp_course_skill_info`

**Schema:**
- learning_path_name
- course_name
- course_level_str
- tutorial_title
- summary
- description
- technologies
- courseMeta
- courseInfo

**Use Cases:**
- Input for recommendation system training
- Course-to-skill mapping
- Learning path analysis
- Personalized learning recommendations

## 🛠️ Technology Stack

- **Python 3.12.1**
- **Pandas** - Data manipulation and analysis
- **Jupyter Notebook** - Interactive data exploration

## 📂 Project Structure

```
learning-buddy-data-ml/
├── model/
├── data/
│   ├── LP and Course Mapping - LP + Course.csv
│   ├── LP and Course Mapping - Course Level.csv
│   ├── LP and Course Mapping - Course.csv
│   ├── LP and Course Mapping - Learning Path.csv
│   ├── LP and Course Mapping - Tutorials.csv
│   ├── Resource Data Learning Buddy - Current Interest Questions.csv
│   ├── Resource Data Learning Buddy - Current Tech Questions.csv
│   ├── Resource Data Learning Buddy - Learning Path Answer (1).csv
│   ├── Resource Data Learning Buddy - Skill Keywords.csv
│   ├── Resource Data Learning Buddy - Student Progress.csv
│   └── output/
│       └── lp_course_skill_info
├── Data-Understanding-Preparation.ipynb
└── README.md
```

## 🚀 Usage

1. **Install dependencies:**
   ```bash
   pip install pandas
   ```

2. **Run the notebook:**
   - Open `Data-Understanding-Preparation.ipynb` in Jupyter
   - Execute cells sequentially to explore and prepare data

3. **Access prepared data:**
   - Load from `data/output/lp_course_skill_info`

## 📊 Key Metrics

- **Total Records:** 7,916+
- **Unique Learning Paths:** 9
- **Learning Path Groups:** Multiple difficulty levels
- **Data Completeness:** 100% (no missing values)
- **Technology Tags:** 50+ unique technologies across courses

## 🎯 Next Steps

1. **Skill Extraction:** Parse technologies field into structured skills
2. **Recommendation System:** Build collaborative filtering or content-based recommender
3. **Student Progress Analysis:** Integrate student progress data for personalized recommendations
4. **Course Difficulty Calibration:** Validate difficulty levels against student performance

## 📝 Notes

- The dataset is enriched with skill information from Learning Buddy Resource database
- All text fields are truncated at 100 characters for display (see `pd.set_option('display.max_colwidth', 100)`)
- The merge operation matches courses between datasets to ensure data consistency
