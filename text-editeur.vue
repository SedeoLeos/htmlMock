<template>
  <div>
    <div id="editor" contenteditable="true" @input="handleInput">
      Bonjour, je suis un éditeur de texte.
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    handleInput(event) {
      const regex = /#[^\s]+/g;
      const regexStrong = /<strong>(\S+)<\/strong>/g;

      let content = event.target.textContent;
      if (content.match(regex) != null) {
        const elementCustom = content.match(regex).map(match => match.slice(1));

        elementCustom.forEach(data => {
          const regexToReplace = new RegExp(`#${data}`);
          content = content.replace(regexToReplace, `<strong>#${data}</strong>`);
          event.target.innerHTML = content;
          if (document.createRange) {
            const range = document.createRange();
            range.selectNodeContents(event.target);
            range.collapse(false);
            const selection = window.getSelection();
            selection.removeAllRanges();
            selection.addRange(range);
          }
        });
      }
    },
  },
};
</script>

<style>
  strong {
    color: rgb(99, 127, 202);
    font-size: x-large;
  }

  #editor {
    font-family: Arial, sans-serif;
    font-size: 16px;
    line-height: 1.5;
    background-color: #f2f2f2;
    padding: 10px;
    border: 0.5px solid #d5eef1;
    border-radius: 3px;
    outline: none;
    height: 200px;
  }
</style>
