- Document.querySelector('.modal') - Choosing a DOM element with the class name and needs a function
- Document.getElementByID('score—1') - Choosing a DOM element by ID
- .textElement - Changes the text element
- .classList.add - adds class to element
- .classList.remove - removes class to element
- .classList.toggle - looks to see if the class is there then removes it, or adds if it's not there
- .addEventListener('click') - Document "listens" for an action like a button click, keydown. The function can include an event so that a key can be specified

```javascript
const modal = document.querySelector('.modal');
const overlay = document.querySelector('.overlay');
const btnCloseModal = document.querySelector('.close-modal');
const btnsOpenModal = document.querySelectorAll('.show-modal');

 const closeModal = function () {
  modal.classList.add('hidden');
  overlay.classList.add('hidden');
};

 const openModal = function () {
  modal.classList.remove('hidden');
  overlay.classList.remove('hidden');
};

 for (let i = 0; i < btnsOpenModal.length; i++) {
  btnsOpenModal[i].addEventListener('click', openModal);
}
btnCloseModal.addEventListener('click', closeModal);
overlay.addEventListener('click', closeModal);
 document.addEventListener('keydown', function (evt) {
  console.log(evt.key);
   if (evt.key === 'Escape') {
    if (!modal.classList.contains('hidden')) {
      closeModal();
    }
  }
});
```

