# PostgreSQL সম্পর্কিত প্রশ্নোত্তর

## 1️⃣ What is PostgreSQL?

**PostgreSQL** একটি শক্তিশালী, ওপেন-সোর্স **রিলেশনাল ডাটাবেস ম্যানেজমেন্ট সিস্টেম (RDBMS)**। এটি ডেটা সংরক্ষণ, অনুসন্ধান, এবং বিশ্লেষণের জন্য ব্যবহৃত হয় এবং ACID কমপ্লায়েন্স নিশ্চিত করে, যা নিরাপদ ও নির্ভরযোগ্য ট্রানজ্যাকশন পরিচালনা করে।

---

## 3️⃣ Explain the Primary Key and Foreign Key concepts in PostgreSQL.

### Primary Key:

Primary Key হলো এমন একটি কলাম (বা কলামগুলোর সমষ্টি) যা প্রতিটি রেকর্ডকে ডাটাবেজের টেবিলে **অদ্বিতীয়ভাবে শনাক্ত** করে।

বৈশিষ্ট্য:

- প্রতিটি ভ্যালু ইউনিক হতে হবে
- `NULL` গ্রহণ করে না

**উদাহরণ:**

```sql
CREATE TABLE species (
  species_id SERIAL PRIMARY KEY,
  common_name TEXT,
  scientific_name TEXT
);
```

### Foreign Key:

Foreign Key এমন একটি কলাম যা অন্য একটি টেবিলের Primary Key এর সাথে সংযুক্ত থাকে, যার মাধ্যমে টেবিলগুলোর মধ্যে সম্পর্ক তৈরি হয়।

বৈশিষ্ট্য:

- Foreign Key হলো একটি কলাম (বা কলামের সেট) যা অন্য টেবিলের Primary Key কে রেফার করে।
- Foreign Key কলামে থাকা মান অবশ্যই রেফার করা টেবিলের Primary Key তে থাকা মানের মধ্যে হতে হবে, নাহলে ডাটাবেজ error দিবে।
- Foreign Key এর মাধ্যমে **অন্য টেবিলের ডেটার উপর নির্ভরশীলতা** (dependency) তৈরি হয়।
- Foreign Key থাকা টেবিলে এমন ডেটা ঢোকানো যায় না যা রেফার করা টেবিলের সাথে মেলে না।

**উদাহরণ:**

```sql
CREATE TABLE sightings (
  sighting_id SERIAL PRIMARY KEY,
  species_id INT REFERENCES species(species_id),
  sighting_time TIMESTAMP
);
```

---

## 5️⃣ Explain the purpose of the WHERE clause in a SELECT statement.

WHERE ক্লজ ব্যবহার করা হয় নির্দিষ্ট শর্ত অনুযায়ী ডেটা ফিল্টার করার জন্য।

উদাহরণ:

```sql
SELECT * FROM sightings
WHERE species_id = 1;
```

এখানে species_id = 1 যেসব রেকর্ডে রয়েছে শুধু সেগুলোই দেখা যাবে।

---

## 7️⃣ How can you modify data using UPDATE statements?

UPDATE কমান্ড দিয়ে বিদ্যমান ডেটা পরিবর্তন করা হয়।

উদাহরণ:

```sql
UPDATE species
SET conservation_status = 'Historic'
WHERE EXTRACT(YEAR FROM discovery_date) < 1800;
```

---

## 9️⃣ Explain the GROUP BY clause and its role in aggregation operations.

GROUP BY ক্লজ ব্যবহার করা হয় একই ভ্যালু ভিত্তিক গ্রুপিং করে সারাংশ (summary) ফলাফল পাওয়ার জন্য। এটি সাধারণত Aggregate ফাংশন (যেমন COUNT, SUM, AVG) এর সাথে ব্যবহৃত হয়।

উদাহরণ:

```sql
SELECT species_id, COUNT(*) AS total_sightings
FROM sightings
GROUP BY species_id;
```

এখানে প্রতিটি species_id অনুযায়ী কয়টি সাইটিং হয়েছে তা গণনা করা হচ্ছে।
