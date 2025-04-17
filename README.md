<script>
    // عند تحميل الصفحة: عرض الطلاب من localStorage
    window.onload = function () {
        const savedStudents = JSON.parse(localStorage.getItem('students')) || [];
        if (savedStudents.length > 0) {
            document.querySelector('.no-students')?.remove();
            savedStudents.forEach(student => {
                addStudentToList(student.name, student.age, student.grade);
            });
        }
    };

    // إضافة طالب إلى القائمة والعرض
    function addStudentToList(name, age, grade) {
        const studentsList = document.getElementById('studentsList');

        const studentItem = document.createElement('li');
        studentItem.textContent = `اسم الطالب: ${name} - العمر: ${age} - الصف: ${grade}`;

        const deleteButton = document.createElement('button');
        deleteButton.textContent = 'مسح';
        deleteButton.classList.add('delete-button');
        deleteButton.addEventListener('click', function () {
            studentsList.removeChild(studentItem);
            updateLocalStorage();
            if (!studentsList.children.length) {
                showNoStudentsMessage();
            }
        });

        studentItem.appendChild(deleteButton);
        studentsList.appendChild(studentItem);
    }

    // تحديث localStorage بعد الإضافة أو الحذف
    function updateLocalStorage() {
        const students = [];
        const listItems = document.querySelectorAll('#studentsList li');

        listItems.forEach(li => {
            if (!li.classList.contains('no-students')) {
                const parts = li.firstChild.textContent.split(' - ');
                const name = parts[0].split(': ')[1];
                const age = parts[1].split(': ')[1];
                const grade = parts[2].split(': ')[1];
                students.push({ name, age, grade });
            }
        });

        localStorage.setItem('students', JSON.stringify(students));
    }

    // عرض رسالة "لا توجد أسماء" إذا القائمة فارغة
    function showNoStudentsMessage() {
        const studentsList = document.getElementById('studentsList');
        const noStudents = document.createElement('li');
        noStudents.className = 'no-students';
        noStudents.textContent = 'لا توجد أي أسماء مسجلة بعد.';
        studentsList.appendChild(noStudents);
    }

    // عند الضغط على زر التسجيل
    document.getElementById('registrationForm').addEventListener('submit', function (event) {
        event.preventDefault();

        const name = document.getElementById('name').value.trim();
        const age = document.getElementById('age').value;
        const grade = document.getElementById('grade').value;

        if (name === '') {
            alert('الرجاء إدخال اسم الطالب.');
            return;
        }

        addStudentToList(name, age, grade);
        updateLocalStorage();
        document.getElementById('registrationForm').reset();
        document.querySelector('.no-students')?.remove();
    });

    // عند الضغط على زر "مسح جميع الطلاب"
    document.getElementById('clearListButton').addEventListener('click', function () {
        const studentsList = document.getElementById('studentsList');
        studentsList.innerHTML = '';
        localStorage.removeItem('students');
        showNoStudentsMessage();
    });
</script>
