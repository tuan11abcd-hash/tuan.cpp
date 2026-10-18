#include <stdio.h>
#include <string.h>

#define MAX 100

typedef struct {
    char id[20];
    char name[50];
    float gpa;
    int credits;
    int discipline;
} Student;

/* Kiểm tra đủ điều kiện học bổng */
int isEligible(Student s) {
    return (s.gpa >= 3.20 &&
            s.credits >= 24 &&
            s.discipline == 0);
}

/* Hàm so sánh phục vụ sắp xếp */
int compare(Student a, Student b) {
    if (a.gpa != b.gpa)
        return a.gpa < b.gpa;

    if (a.credits != b.credits)
        return a.credits < b.credits;

    return strcmp(a.id, b.id) > 0;
}

/* Bubble Sort */
void bubbleSort(Student a[], int n) {
    int i, j;
    Student temp;

    for (i = 0; i < n - 1; i++) {
        for (j = 0; j < n - i - 1; j++) {
            if (compare(a[j], a[j + 1])) {
                temp = a[j];
                a[j] = a[j + 1];
                a[j + 1] = temp;
            }
        }
    }
}

/* In danh sách */
void printStudents(Student a[], int n) {
    printf("\n%-10s %-20s %-6s %-8s\n",
           "ID", "Name", "GPA", "Credits");

    for (int i = 0; i < n; i++) {
        printf("%-10s %-20s %-6.2f %-8d\n",
               a[i].id,
               a[i].name,
               a[i].gpa,
               a[i].credits);
    }
}

int main() {

    Student students[] = {
        {"SV08", "Nguyen Van H", 3.70, 30, 0},
        {"SV02", "Tran Thi B",   3.85, 28, 0},
        {"SV05", "Le Van E",     3.70, 33, 0},
        {"SV01", "Nguyen Van A", 3.20, 25, 0},
        {"SV06", "Pham Thi F",   3.70, 33, 0},
        {"SV07", "Hoang Van G",  3.95, 22, 0},
        {"SV03", "Le Thi C",     3.10, 27, 0},
        {"SV04", "Pham Van D",   3.90, 32, 1}
    };

    int total = sizeof(students) / sizeof(students[0]);

    Student eligible[MAX];
    int count = 0;

    /* Lọc sinh viên đủ điều kiện */
    for (int i = 0; i < total; i++) {
        if (isEligible(students[i])) {
            eligible[count++] = students[i];
        }
    }

    /* Sắp xếp */
    bubbleSort(eligible, count);

    printf("===== DANH SACH DU DIEU KIEN HOC BONG =====\n");
    printStudents(eligible, count);

    printf("\nTong ho so: %d\n", total);
    printf("Du dieu kien: %d\n", count);
    printf("Khong du dieu kien: %d\n", total - count);

    printf("\n===== TOP 3 NHAN HOC BONG =====\n");

    int top = (count < 3) ? count : 3;

    for (int i = 0; i < top; i++) {
        printf("%d. %s - %s (GPA %.2f)\n",
               i + 1,
               eligible[i].id,
               eligible[i].name,
               eligible[i].gpa);
    }

    return 0;
}
