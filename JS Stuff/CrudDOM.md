# CrudDOM - JS Learning

## Task

**1.** Create a simple Book List application

- Book List app is a simple web app that allows users to add books to a list
- Each book has **Title**, **Author**, and the **year** it was published

## Solution

```jsx
const title = document.getElementById('title');
const author = document.getElementById('author');
const year = document.getElementById('year');
const bookList = document.getElementById('book-list');
const btn = document.querySelector('.btn');

function addBookToList(title, author, year) {
  const BookEntry = document.createElement('section');
  BookEntry.classList.add("book-entry");
  
  const titleDiv = document.createElement('div');
  titleDiv.textContent = title.value;
  BookEntry.appendChild(titleDiv);
  
  const authorDiv = document.createElement('div');
  authorDiv.textContent = author.value;
  BookEntry.appendChild(authorDiv);
  
  const yearDiv = document.createElement('div');
  yearDiv.textContent = year.value;
  BookEntry.appendChild(yearDiv);
  
  return BookEntry;
}

btn.addEventListener('click', function (e) {
  e.preventDefault();
  bookList.appendChild(addBookToList(title, author, year));
});

```