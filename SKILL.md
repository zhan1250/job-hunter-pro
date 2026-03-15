---
name: job-hunter-pro
description: "Professional job hunting automation for Chinese job market. Search, filter, and organize job listings from multiple platforms (Liepin, Zhaopin, LinkedIn, Boss) with intelligent deduplication and structured output."
author: luna-assistant
version: 1.0.0
---

# Job Hunter Pro

> **Professional job hunting automation for the Chinese job market.**

This skill automates the complete job search workflow: multi-platform search, intelligent filtering, company deduplication, and structured output generation.

## 🌟 Features

- **Multi-Platform Search**: Aggregates jobs from Liepin, Zhaopin, LinkedIn, Boss Zhipin, and 51job
- **Smart Filtering**: Location-based filtering (Shanghai downtown only), company type, experience level
- **Intelligent Deduplication**: Real-time company blacklist checking to avoid duplicates
- **Structured Output**: Generates professional CSV/Excel with 11 standardized columns
- **Strict Ratio Control**: Enforces 5-5-5-5 distribution across 4 job categories
- **Browser Automation**: Uses Chrome automation for reliable data extraction

## 🎯 Job Categories (5-5-5-5 Rule)

| Category | Count | Keywords | Priority |
|:---|:---:|:---|:---:|
| **Data Analyst** | 5 | 数据分析师、数据分析专员、商业数据分析 | ⭐⭐⭐⭐⭐ |
| **Trade Specialist** | 5 | 贸易专员、大宗商品交易员、进出口运营 | ⭐⭐⭐⭐ |
| **Foreign Trade** | 5 | 外贸业务员、外贸销售、国际业务专员 | ⭐⭐⭐ |
| **AI/Creative** | 5 | AIGC、AI产品经理、内容运营、漫剧创作 | ⭐⭐⭐ |

**Total**: Exactly 20 jobs per day, strictly 5+5+5+5 distribution

## 📊 Platform Distribution

| Platform | Percentage | Count | URL |
|:---|:---:|:---:|:---|
| 智联招聘 (Zhaopin) | 25% | 5 | zhaopin.com |
| LinkedIn | 25% | 5 | linkedin.com/jobs |
| 猎聘网 (Liepin) | 25% | 5 | liepin.com |
| Boss直聘 + 前程无忧 | 25% | 5 | zhipin.com + 51job.com |

## 🛠️ Tools

### `search_jobs`

Searches for jobs across multiple platforms with intelligent filtering.

**Parameters:**
- `category` (string): Job category - "data_analyst", "trade_specialist", "foreign_trade", "ai_creative"
- `count` (number): Number of jobs to find (default: 5)
- `location` (string): Target location (default: "上海市区")
- `exclude_companies` (array): List of companies to exclude

**Example:**
```json
{
  "category": "data_analyst",
  "count": 5,
  "location": "上海市区",
  "exclude_companies": ["Company A", "Company B"]
}
```

### `generate_job_report`

Generates structured CSV/Excel report from collected jobs.

**Parameters:**
- `jobs` (array): Array of job objects
- `date` (string): Report date (YYYY-MM-DD)
- `format` (string): "csv" or "excel" (default: "excel")

**Output Columns:**
1. 排名 (Rank)
2. 职位名称 (Job Title)
3. 公司名称 (Company Name)
4. 公司类型 (Company Type)
5. 地点 (Location)
6. 经验要求 (Experience)
7. 学历要求 (Education)
8. 薪资范围 (Salary Range)
9. 职位链接 (Job URL) - **Required**
10. 匹配度 (Match Percentage)
11. 备注 (Notes)

### `update_company_blacklist`

Updates the company blacklist with newly found companies.

**Parameters:**
- `companies` (array): List of company names to add

## 📍 Location Filtering

### ✅ Included (Shanghai Downtown)
- **黄浦区、静安区、徐汇区、长宁区**
- **普陀区** (部分)、**虹口区、杨浦区**
- **浦东新区**: 陆家嘴、世纪大道、张江（核心区域）
- **闵行区**: 虹桥、七宝（靠近市区部分）

### ❌ Excluded (Suburbs)
- 嘉定区、青浦区、松江区、奉贤区、金山区
- 宝山区（大部分）、崇明区
- 浦东: 川沙、周浦、南汇、金桥（偏外）
- 闵行: 浦江、莘庄（偏外）

## 🔍 Search Workflow

```
1. Load company blacklist from ~/Desktop/求职/已推荐公司清单.txt
2. For each of 4 categories:
   a. Search platform with category keywords
   b. Extract job details (title, company, location, salary, URL)
   c. Filter by location (downtown only)
   d. Check company against blacklist
   e. Collect until 5 valid jobs per category
3. Generate Excel report with all 20 jobs
4. Update company blacklist
5. Send report to user via Feishu
```

## 📁 File Structure

```
~/Desktop/求职/
├── 已推荐公司清单.txt     # Company blacklist
├── 职位列表_YYYY-MM-DD.xlsx  # Daily job report
├── Gavin Resume 2025.docx    # English resume
└── 张嘉文简历2025.pdf       # Chinese resume
```

## 💼 Company Types

- **外企** (Foreign Enterprise)
- **民企** (Private Enterprise)
- **国企** (State-owned Enterprise)
- **上市公司** (Listed Company)

## ⚠️ Important Rules

1. **Strict 5-5-5-5 Distribution**: Never deviate from the ratio
2. **Job URL Required**: Every job must include a valid application link
3. **Location Filter**: Only Shanghai downtown, no suburbs
4. **Real-time Deduplication**: Check blacklist before adding each job
5. **Browser-First**: Use browser automation, not Brave Search API
6. **Daily Limit**: Exactly 20 jobs, no more, no less

## 📦 Installation

```bash
# Using OpenClaw CLI
openclaw install skill https://github.com/your-username/job-hunter-pro

# Or using ClawHub
clawhub install job-hunter-pro
```

## 🔧 Configuration

Create `~/.openclaw/job-hunter-pro/config.json`:

```json
{
  "target_location": "上海市区",
  "daily_target": 20,
  "category_distribution": {
    "data_analyst": 5,
    "trade_specialist": 5,
    "foreign_trade": 5,
    "ai_creative": 5
  },
  "platforms": [
    "zhaopin",
    "linkedin",
    "liepin",
    "zhipin",
    "51job"
  ],
  "output_path": "~/Desktop/求职/",
  "blacklist_file": "~/Desktop/求职/已推荐公司清单.txt"
}
```

## 🚀 Usage Examples

### Daily Job Search
```
User: "搜索职位"
Agent: 
  1. Load blacklist
  2. Search 4 categories (5 jobs each)
  3. Generate Excel report
  4. Send via Feishu
```

### Specific Category Search
```
User: "再找5个数据分析师职位"
Agent:
  1. Search data_analyst category
  2. Filter by location
  3. Check blacklist
  4. Return 5 jobs
```

### Generate Report
```
User: "生成昨天的职位报告"
Agent:
  1. Read collected jobs from memory
  2. Generate formatted Excel
  3. Save to ~/Desktop/求职/
```

## 📝 Output Example

**Excel File**: `职位列表_2026-03-15.xlsx`

| 排名 | 职位名称 | 公司名称 | 公司类型 | 地点 | 经验要求 | 学历要求 | 薪资范围 | 职位链接 | 匹配度 | 备注 |
|:---:|:---|:---|:---:|:---|:---:|:---:|:---:|:---|:---:|:---|
| 1 | 数据分析师 | ABC科技 | 外企 | 静安区 | 3-5年 | 本科 | 25-35K | [链接](...) | 95% | 匹配度高 |
| 2 | 贸易专员 | XYZ贸易 | 民企 | 黄浦区 | 1-3年 | 本科 | 15-25K | [链接](...) | 90% | - |
| ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |

## 🔗 Dependencies

- **browser-automation**: For web scraping
- **multi-search-engine**: Fallback search
- **excel-automation**: For report generation
- **feishu-doc**: For sending reports

## 📄 License

MIT License - Free for personal and commercial use.

---

*Optimized for the Chinese job market | Created by Luna Assistant*