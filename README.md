
import sqlite3

def init_db():
    conn = sqlite3.connect("police_ledger.db")
    cursor = conn.cursor()

    cursor.execute('''
    CREATE TABLE IF NOT EXISTS Officers (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT,
        rank TEXT,
        badge_number TEXT UNIQUE
    )
    ''')

    cursor.execute('''
    CREATE TABLE IF NOT EXISTS Complaints (
        complaint_id INTEGER PRIMARY KEY AUTOINCREMENT,
        complainant_name TEXT,
        contact_info TEXT,
        date TEXT,
        description TEXT,
        officer_id INTEGER,
        FOREIGN KEY(officer_id) REFERENCES Officers(id)
    )
    ''')

    cursor.execute('''
    CREATE TABLE IF NOT EXISTS FIRs (
        fir_id INTEGER PRIMARY KEY AUTOINCREMENT,
        complaint_id INTEGER,
        date_filed TEXT,
        status TEXT,
        FOREIGN KEY(complaint_id) REFERENCES Complaints(complaint_id)
    )
    ''')

    cursor.execute('''
    CREATE TABLE IF NOT EXISTS Visitors (
        visitor_id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT,
        purpose TEXT,
        visit_date_time TEXT
    )
    ''')

    cursor.execute('''
    CREATE TABLE IF NOT EXISTS Evidence (
        evidence_id INTEGER PRIMARY KEY AUTOINCREMENT,
        complaint_id INTEGER,
        description TEXT,
        stored_location TEXT,
        FOREIGN KEY(complaint_id) REFERENCES Complaints(complaint_id)
    )
    ''')

    conn.commit()
    conn.close()

if __name__ == "__main__":
    init_db()
    print("Database and tables created successfully.")
