<script>
    export default {
        name: "TextEditor",

        /**
         * Входные параметры
         * компонента
         */
        props: [
            "text",
            "active",
            "autoresize",
            "placeholder"
        ],

        /**
         * Методы компонента
         */
        methods: {
            /**
             * Автоматическое изменение размера
             * в зависимости от величины контента
             */
            autoResize(e) {
                if (!this.autoresize) {
                    return;
                }

                e.target.style.height = 'auto';
                e.target.style.height = `${e.target.scrollHeight}px`;
            },

            /**
             * Горячие клавиши форматирования
             * текста
             */
            shortCutes (e) {
                if (e.ctrlKey) {
                    switch(e.keyCode) {
                        // CTRL + ArrowUp - верхний индекс
                        case 38:
                            document.execCommand('superscript', false, null);
                            break;

                        // CTRL + ArrowDown - нижний индекс
                        case 40:
                            document.execCommand('subscript', false, null);
                            break;

                        // CTRL + Enter - маркерованный список
                        // CTRL + SHIFT + Enter - нумерованный список
                        case 13:
                            if (e.shiftKey) {
                                document.execCommand('insertOrderedList', false, null);
                            } else {
                                document.execCommand('insertUnorderedList', false, null);
                            }
                            
                            break;

                        // CTRL + ALT + R - выравнять текст по правому краю
                        case 82: 
                            if (e.altKey) {
                                document.execCommand('justifyRight', false, null);
                            }
                            break;

                        // CTRL + ALT + L - выравнять текст по левому краю
                        case 76:
                            if (e.altKey) {
                                document.execCommand('justifyLeft', false, null);
                            }
                            break;

                        // CTRL + ALT + F - выравнять текст по ширине
                        case 70:
                            if (e.altKey) {
                                document.execCommand('justifyFull', false, null);
                            }
                            break;

                        // CTRL + ALT + C - выравнять текст по центру
                        case 67:
                            if (e.altKey) {
                                document.execCommand('justifyCenter', false, null);
                            }
                            break;
                    }
                } else {
                    // Включение табуляции
                    if (e.keyCode === 9) {
                        e.preventDefault();
                        if (!e.shiftKey) {
                            document.execCommand('insertText', false, "\t");
                        } else {
                            const doc = e.target.ownerDocument.defaultView;
                            const sel = doc.getSelection();
                            const range = sel.getRangeAt(0);
                            if (/(\t)/.test(range.startContainer.data)) {
                                document.execCommand('delete');
                            }
                        }
                    }
                }
            }
        }
    }
</script>

<template>
    <pre 
        :placeholder="placeholder"
        v-text="text"
        :contenteditable="!!active"
        :class="{
            active: !!active
        }"
        class="text-editor-wrapper" 
        @change="autoResize"
        @keydown.stop="shortCutes"
    />
</template>

<style>
    .text-editor-wrapper {
        outline: 0;

        padding: 8px;

        tab-size: 4;

        border-radius: 4px;
        background-color: transparent;
        transition: background-color 0.3s;
    }

    .text-editor-wrapper.active[placeholder]:empty:before {
        content: attr(placeholder);
        color: #555;
    }

    .text-editor-wrapper.active {
        background-color: #eee;
    }
</style>