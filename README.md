import pandas as pd

data = [
["2026CS001","Aditi","Computer Engineering",92,12,8,84,88],
["2026CS002","Bhavana","Computer Engineering",64,6,5,58,61],
["2026IT001","Chaitra","Information Technology",78,9,7,72,76],
["2026IT002","Divya","Information Technology",55,4,3,46,52],
["2026CE001","Esha","Civil Engineering",88,10,8,79,82],
["2026CE002","Fatima","Civil Engineering",71,8,6,66,69],
["2026CS003","Gauri","Computer Engineering",96,14,9,91,94],
["2026IT003","Harshada","Information Technology",43,2,2,35,41]
]

columns = [
"student_id",
"name",
"department",
"attendance_pct",
"diary_entries",
"tasks_completed",
"assignment_score",
"lab_score"
]

df = pd.DataFrame(data, columns=columns)

df.to_csv(
"week_2_3_exam_student_signals.csv",
index=False
)

print(df.head())

# Check Data
print(df.info())
print(df.isnull().sum())
print("Shape:", df.shape)

def readiness(row):
    score = (
        row["attendance_pct"]*0.25 +
        row["diary_entries"]*2 +
        row["tasks_completed"]*3 +
        row["assignment_score"]*0.20 +
        row["lab_score"]*0.25
    )
    return round(score,2)

df["readiness_score"] = df.apply(readiness, axis=1)

def classify(score):
    if score < 50:
        return "Support"
    elif score < 70:
        return "Developing"
    elif score < 90:
        return "Strong"
    else:
        return "Excellent"

df["band"] = df["readiness_score"].apply(classify)


print("\nStudent Results")
print(df[["name","readiness_score","band"]])

print("\nDepartment Average")
print(df.groupby("department")["readiness_score"].mean())

print("\nTop Performer")
print(df.loc[df["readiness_score"].idxmax()])

print("\nStudents Needing Support")
print(df[df["band"]=="Support"])

