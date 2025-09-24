# 📖 Vatican Word of the Day API

[![Java](https://img.shields.io/badge/Java-17+-blue.svg)](https://openjdk.java.net/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![API Status](https://img.shields.io/badge/API-Live-brightgreen.svg)](#public-api)

An open-source service that provides daily Vatican Word of the Day readings, including biblical readings, gospel passages, and Pope's commentary. Perfect for integrating Catholic content into websites, mobile apps, or devotional applications.

## 🌟 Features

- 📅 **Daily Readings**: Automatically updated Vatican Word of the Day content
- 📖 **Complete Content**: Reading of the Day, Gospel, and Pope's Words
- 🔗 **Public API**: Easy integration via Google Sheets CSV export
- 📊 **Historical Data**: Access to past readings by date
- 🌐 **Cross-Platform**: Works with any programming language via CSV
- 🛡️ **Reliable**: Automated scraping with error handling

## 🚀 Public API

### Live Data Access
Get the latest Vatican Word of the Day readings directly from our published spreadsheet:

**📊 CSV Endpoint:**
```
https://docs.google.com/spreadsheets/d/e/2PACX-1vTou7QCtMbPeRj5g-G1uNPMr5alQ-SbLlf7ToXfGkScywPqawLNnfVi7kr8ns0GibG4Mqkdhovxe_7j/pub?output=csv
```

### Data Format
Each row contains:
| Column | Content |
|--------|---------|
| **Date** | YYYY-MM-DD format |
| **Reading of the Day** | Biblical reading passage |
| **Gospel of the Day** | Gospel passage |
| **The Words of the Popes** | Pope's commentary |

### Integration Examples

#### JavaScript/HTML
```html
<!DOCTYPE html>
<html>
<head>
    <title>Daily Vatican Reading</title>
</head>
<body>
    <div id="reading"></div>
    
    <script>
        async function loadTodayReading() {
            const response = await fetch('https://docs.google.com/spreadsheets/d/e/2PACX-1vTou7QCtMbPeRj5g-G1uNPMr5alQ-SbLlf7ToXfGkScywPqawLNnfVi7kr8ns0GibG4Mqkdhovxe_7j/pub?output=csv');
            const csvText = await response.text();
            
            // Parse CSV and find today's reading
            const lines = csvText.split('\n');
            const today = new Date().toISOString().split('T')[0]; // YYYY-MM-DD
            
            for (let i = 1; i < lines.length; i++) {
                const [date, reading, gospel, pope] = lines[i].split(',');
                if (date === today) {
                    document.getElementById('reading').innerHTML = `
                        <h2>Reading of the Day</h2>
                        <p>${reading}</p>
                        <h2>Gospel</h2>
                        <p>${gospel}</p>
                        <h2>Pope's Words</h2>
                        <p>${pope}</p>
                    `;
                    break;
                }
            }
        }
        
        loadTodayReading();
    </script>
</body>
</html>
```

#### Python
```python
import requests
import csv
from datetime import datetime

def get_todays_reading():
    url = "https://docs.google.com/spreadsheets/d/e/2PACX-1vTou7QCtMbPeRj5g-G1uNPMr5alQ-SbLlf7ToXfGkScywPqawLNnfVi7kr8ns0GibG4Mqkdhovxe_7j/pub?output=csv"
    response = requests.get(url)
    
    csv_data = csv.reader(response.text.splitlines())
    next(csv_data)  # Skip header
    
    today = datetime.now().strftime("%Y-%m-%d")
    
    for row in csv_data:
        if row[0] == today:
            return {
                'date': row[0],
                'reading': row[1],
                'gospel': row[2],
                'pope_words': row[3]
            }
    
    return None

# Usage
reading = get_todays_reading()
if reading:
    print(f"Today's Reading: {reading['reading']}")
```

#### PHP
```php
<?php
function getTodaysReading() {
    $url = "https://docs.google.com/spreadsheets/d/e/2PACX-1vTou7QCtMbPeRj5g-G1uNPMr5alQ-SbLlf7ToXfGkScywPqawLNnfVi7kr8ns0GibG4Mqkdhovxe_7j/pub?output=csv";
    $csv = array_map('str_getcsv', file($url));
    
    $today = date('Y-m-d');
    
    foreach ($csv as $row) {
        if ($row[0] === $today) {
            return [
                'date' => $row[0],
                'reading' => $row[1],
                'gospel' => $row[2],
                'pope_words' => $row[3]
            ];
        }
    }
    
    return null;
}

// Usage
$reading = getTodaysReading();
if ($reading) {
    echo "<h2>Reading of the Day</h2>";
    echo "<p>" . $reading['reading'] . "</p>";
}
?>
```

## 🛠️ Self-Hosted Setup

Want to run your own scraper? Follow these steps:

### Prerequisites
- **Java 17+** (JDK)
- **Google Cloud Project** with Google Sheets API enabled
- **Service Account JSON key**

### Quick Start
1. **Clone the repository:**
   ```bash
   git clone https://github.com/yourusername/vatican-wotd-scraper.git
   cd vatican-wotd-scraper
   ```

2. **Setup Google Sheets API:**
   - Go to [Google Cloud Console](https://console.cloud.google.com/)
   - Enable Google Sheets API
   - Create Service Account and download JSON key
   - Rename to `service_account.json` and place in project root

3. **Configure:**
   ```bash
   cp config.properties.example config.properties
   # Edit config.properties with your Google Sheet ID
   ```

4. **Run the scraper:**
   ```bash
   ./gradlew run
   ```

## 📊 Google Spreadsheet Access

### Public Spreadsheet
View the live data in Google Sheets:
**🔗 [Open Vatican Word of the Day Spreadsheet](https://docs.google.com/spreadsheets/d/1LqgabFsJYJx9FC5CnyqM0i9tlDULV7a9bhUS0E3K-QI/edit#gid=0)**

### CSV Export URL
For programmatic access:
```
https://docs.google.com/spreadsheets/d/e/2PACX-1vTou7QCtMbPeRj5g-G1uNPMr5alQ-SbLlf7ToXfGkScywPqawLNnfVi7kr8ns0GibG4Mqkdhovxe_7j/pub?output=csv
```

## 🎯 Use Cases

- **📱 Mobile Apps**: Catholic devotional apps
- **🌐 Websites**: Parish websites, Catholic blogs
- **📧 Newsletters**: Daily email devotions
- **🤖 Chatbots**: Catholic assistant bots
- **📚 Study Tools**: Bible study applications
- **⛪ Church Displays**: Digital signage systems

## 📝 API Documentation

### Endpoints
- **CSV Data**: `GET /pub?output=csv` - Returns all readings as CSV
- **Date Filtering**: Parse CSV and filter by date column

### Response Format
```csv
Date,Reading of the Day,Gospel of the Day,The Words of the Popes
2025-09-22,"A reading from the Book of Ezra...","From the Gospel according to Luke...","Do not conceal the good for tomorrow..."
```

### Rate Limits
- No rate limits on the public CSV endpoint
- Data updates daily via automated scraping
- Historical data available for past dates

## 🤝 Contributing

We welcome contributions! Here's how you can help:

1. **🐛 Report Issues**: Found a bug? Open an issue
2. **💡 Feature Requests**: Suggest new features
3. **🔧 Code Contributions**: Submit pull requests
4. **📚 Documentation**: Improve documentation
5. **🌍 Translations**: Help with multilingual support

### Development Setup
```bash
git clone https://github.com/yourusername/vatican-wotd-scraper.git
cd vatican-wotd-scraper
./gradlew build
./gradlew test
```

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## ⚠️ Disclaimer

- This service is **not affiliated** with the Vatican or Vatican News
- Content is scraped from publicly available Vatican News pages
- Please respect the Vatican News website's terms of service
- Use responsibly and in accordance with Catholic teachings

## 🙏 Acknowledgments

- **Vatican News** for providing the Word of the Day content
- **Contributors** who help maintain and improve this service
- **Catholic developers** building amazing applications with this data

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/yourusername/vatican-wotd-scraper/issues)
- **Discussions**: [GitHub Discussions](https://github.com/yourusername/vatican-wotd-scraper/discussions)
- **Email**: your-email@example.com

---

**Made with ❤️ for the Catholic developer community**