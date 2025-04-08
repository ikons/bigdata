# 🔥 Big Data με Apache Spark, HDFS, Docker & Kubernetes

Αυτό το αποθετήριο περιέχει κώδικα, δεδομένα και οδηγίες για την εκτέλεση εργασιών **Apache Spark** με **RDDs**, **DataFrames** και **Map/Reduce** με χρήση **τοπικής (Docker)** και **κατανεμημένης (Kubernetes)** υποδομής για το μάθημα [Συστήματα Διαχείρισης Μεγάλου Όγκου Δεδομένων](https://eclass.uth.gr/courses/CS_U_218/) του [Τμήματος Πληροφορικής και Τηλεπικοινωνιών](http://www.dit.uth.gr) [Πανεπιστημίου Θεσσαλίας](http://www.uth.gr).

---

## 📘 Σειρά Μελέτης / Οδηγίες Εκτέλεσης (Markdown)

1. [00_Preparatory-lab](docs/00_Preparatory-lab): Προετοιμασία περιβάλλοντος (WSL + Docker Desktop)
2. [00_pycharm](docs/00_pycharm): Εκτέλεση Spark τοπικά με PyCharm
3. [01_lab1-docker](docs/01_lab1-docker): Εκκίνηση Spark + HDFS μέσω Docker Compose
4. [01_lab1-k8s](docs/01_lab1-k8s): Εκτέλεση Spark Jobs σε Kubernetes (vdcloud)
5. [02_lab2](docs/02_lab2): Εκτέλεση ερωτημάτων συνένωσης με χρήση RDD,  DataFrames και SQL

📁 Εναλλακτικά, όλοι οι οδηγοί είναι διαθέσιμοι και στον φάκελο [`odigoi/`](./odigoi) σε μορφή `.docx` και `.pdf`.


---

## 📁 Δομή Αποθετηρίου

- `code/`: Κώδικας Spark σε Python (RDD & DataFrame)
- `examples/`: CSV αρχεία για δοκιμές (employees, departments, text)
- `docker/`
  - `01-lab1-spark-hdfs/`: Spark + HDFS setup με Docker Compose
  - `02-lab2-spark-history-server/`: Spark History Server setup με Docker
- `docs/`: 📘 Όλοι οι οδηγοί σε μορφή Markdown
- `odigoi/`: 🧾 Οδηγοί σε `.docx` και `.pdf`

---



## 💻 Εκτέλεση με PyCharm (Τοπική Ανάπτυξη)

📄 Οδηγός: [`00_Preparatory-lab`](docs/00_Preparatory-lab)

- Χρήση `venv`, εγκατάσταση `pyspark` & `psutil`
- Ρύθμιση μεταβλητών περιβάλλοντος στο Run Configuration
- Υποστήριξη Spark UI μέσω `localhost:4040`

---

## 🧱 Προετοιμασία Περιβάλλοντος (WSL + Docker Desktop)

📄 Οδηγός: [`docs/00_Preparatory-lab`](docs/00_Preparatory-lab/)

- Εγκατάσταση WSL 2 και Ubuntu
- Ρύθμιση Docker Desktop για χρήση WSL backend
- Επιβεβαίωση εγκατάστασης και τεστ με `hello-world`

---

## 🐳 Lab 01a: Εκτέλεση Spark + HDFS μέσω Docker

📄 Οδηγός: [`01_lab1-docker`](docs/01_lab1-docker)

```bash
cd docker/01-lab1-spark-hdfs
docker compose up -d
```

- Spark UI: http://localhost:8080  
- HDFS NameNode: http://localhost:9870  
- Εκτέλεση παραδείγματος:
```bash
docker exec spark-master spark-submit /mnt/upload/wordcount.py
```

📂 Ανέβασμα αρχείων:  
```
\wsl.localhost\docker-desktop\mnt\... (shared volume path)
```

---

## ☁️ Lab 01b: Spark σε Kubernetes

📄 Οδηγός: [`01_lab1-k8s`](docs/01_lab1-k8s)

- Εκτελεί Spark σε Kubernetes (vdcloud)
- Απαιτεί OpenVPN & χρήση `k9s` για παρακολούθηση

### Παράδειγμα `spark-submit`:

```bash
spark-submit \
    --master k8s://https://<k8s-cluster-endpoint> \
    --deploy-mode cluster \
    --conf spark.kubernetes.container.image=apache/spark \
    hdfs://.../wordcount_localdir.py
```

---

## 🔁 Lab 02: Εκτέλεση ερωτημάτων συνένωσης με την χρήση RDD, DataFrames και SQL

📄 Οδηγός: [`02_lab2`](docs/02_lab2)

Σε αυτό το εργαστήριο υλοποιούνται ερωτήματα συνένωσης πινάκων (joins) τόσο με RDDs όσο και με DataFrames. Περιλαμβάνει την ταξινόμηση και την ομαδοποίηση αποτελεσμάτων, καθώς και χρήση SQL queries.

Στο τέλος του οδηγού, γίνεται και σύντομη αναφορά στην ενεργοποίηση και χρήση του **Spark History Server** για την παρακολούθηση των ερωτημάτων μέσω web UI.

---

## ⚙️ Προετοιμασία Δεδομένων στο HDFS

```bash
# Αντιγραφή των φακέλων στο HDFS
hadoop fs -put examples examples
hadoop fs -put code code

# Επιβεβαίωση
hadoop fs -ls examples
hadoop fs -ls code
```

---

## 🧪 Εκτέλεση Ερωτημάτων Spark

| Ερώτημα       | Περιγραφή                                  | Υλοποίηση |
|---------------|---------------------------------------------|------------|
| Query 1       | 5 υπάλληλοι με τον χαμηλότερο μισθό         | RDD / DF   |
| Query 2       | 3 υψηλόμισθοι υπάλληλοι του "Dep A"         | RDD / DF   |
| Query 3       | Άθροισμα μισθών ανά τμήμα                   | DF         |
| Word Count    | Καταμέτρηση λέξεων σε αρχείο κειμένου       | RDD        |

📈 Τα παραπάνω queries μπορούν να παρακολουθηθούν μέσω του Spark History Server (αναφορά στο τέλος του [02_lab2](docs/02_lab2)).

### Παράδειγμα εκτέλεσης:

```bash
# ⚠️ Αντικατέστησε 👇 το "ikons" με το δικό σου username
spark-submit hdfs://hdfs-namenode:9000/user/ikons/code/RddQ1.py
```

---



---

## 👤 Συντελεστής

**ikons**  
📬 Για απορίες: [GitHub Issues](https://github.com/ikons/bigdata/issues)
