<template>
  <div class="editor">
    <editor-menu-bar :editor="editor" v-slot="{ commands, isActive }">
      <div class="menubar">
        <button
          class="menubar_button"
          :class="{ 'is-active': isActive.heading({ level: 1 }) }"
          @click="commands.heading({ level: 1 })"
        >
          <v-icon name="heading" />
        </button>
        <button
          class="menubar_button"
          :class="{ 'is-active': isActive.bold() }"
          @click="commands.bold"
        >
          <v-icon name="bold" />
        </button>
        <button
          class="menubar_button"
          :class="{ 'is-active': isActive.customstyle({ level: 'body-blue' }) }"
          @click="showOver(showCard)"
        >
          <v-icon name="font" />
          <v-icon name="sort-down" />
        </button>
        <button
          class="menubar_button"
          :class="{ 'is-active': isActive.bullet_list() }"
          @click="commands.bullet_list"
        >
          <v-icon name="list-ul" />
        </button>
        <button
          class="menubar_button"
          :class="{ 'is-active': isActive.ordered_list() }"
          @click="commands.ordered_list"
        >
          <v-icon name="list-ol" />
        </button>
        <button class="menubar_button" @click="showImagePrompt(commands.image)">
          <v-icon name="image" />
        </button>
        <button class="menubar_button" @click="showVideoModal(commands.iframe)">
          <v-icon name="video" />
        </button>
        <div v-show="showCard" class="hover_card">
          <button
            class="color_botton button-blue"
            @click="commands.customstyle({ level: 'body-blue' }), showOver(showCard)"
          ></button>
          <button
            class="color_botton button-red"
            @click="commands.customstyle({ level: 'body-red' }), showOver(showCard)"
          ></button>
          <button
            class="color_botton button-black"
            @click="commands.customstyle({ level: 'body-black' }), showOver(showCard)"
          ></button>
        </div>
      </div>
    </editor-menu-bar>
    <editor-content class="editor_content" :editor="editor" />
    <Modal ref="videoModal" @onConfirm="addCommand" />
  </div>
</template>

<script>
import { Editor, EditorContent, EditorMenuBar } from "tiptap";
import {
  Blockquote,
  CodeBlock,
  HardBreak,
  Heading,
  OrderedList,
  BulletList,
  ListItem,
  TodoItem,
  TodoList,
  Bold,
  Code,
  Italic,
  Link,
  Strike,
  Underline,
  History,
  Image,
  TrailingNode
} from "tiptap-extensions";
import Iframe from "./Iframe";
import Modal from "./Modal";
import CustomStyle from "./CustomStyle";

export default {
  name: "TextEditor",
  components: {
    EditorMenuBar,
    EditorContent,
    Modal
  },
  data() {
    return {
      editor: new Editor({
        extensions: [
          new Blockquote(),
          new CodeBlock(),
          new HardBreak(),
          new Heading({ levels: [1, 2, 3] }),
          new BulletList(),
          new OrderedList(),
          new ListItem(),
          new TodoItem(),
          new TodoList(),
          new Bold(),
          new Code(),
          new Italic(),
          new Link(),
          new Strike(),
          new Underline(),
          new History(),
          new Image(),
          new TrailingNode(),
          new Iframe(),
          new CustomStyle()
        ],
        content: `
          <h1>Yay Headlines!</h1>
          <p>All these <strong>cool tags</strong> are working now.</p>
          <ul>
            <li>111</li>
            <li>222</li>
            <li>333</li>
          </ul>
          <img src="https://i.imgur.com/7i9S826.gif" />
        `
      }),
      showCard: false
    };
  },
  methods: {
    showImagePrompt(command) {
      const src = prompt("Enter the url of your image here");
      if (src !== null) {
        command({ src });
      }
    },
    showVideoModal(command) {
      this.$refs.videoModal.showModal(command);
    },
    addCommand(data) {
      if (data.command !== null) {
        data.command(data.data);
      }
    },
    showOver(showCard) {
      this.showCard = !showCard;
    }
  },
  beforeDestroy() {
    this.editor.destroy();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.editor {
  position: relative;
  max-width: 40rem;
  margin: 0 auto 5rem auto;
  border: 1px solid #939699;
}
.editor_content {
  overflow-wrap: break-word;
  word-wrap: break-word;
  word-break: break-word;
}
.menubar {
  margin: 1rem;
  transition: visibility 0.2s 0.4s, opacity 0.2s 0.4s;
}
.menubar_button {
  font-weight: bold;
  display: inline-flex;
  background: transparent;
  border: 0;
  padding: 0.2rem 0.5rem;
  margin-right: 0.2rem;
  border-radius: 3px;
  cursor: pointer;
}
.is-active {
  background-color: #939699;
}
.hover_card {
  position: relative;
  overflow: hidden;
  border: 1px solid #c0c0c0;
  padding: 10px;
  background: #fff;
  -webkit-box-shadow: 2px 2px 5px 0px rgba(0, 0, 0, 0.75);
  -moz-box-shadow: 2px 2px 5px 0px rgba(0, 0, 0, 0.75);
  box-shadow: 2px 2px 5px 0px rgba(0, 0, 0, 0.75);
  vertical-align: top;
  height: 30px;
  width: 90px;
  z-index: 9999;
  left: 8%;
}
.color_botton {
  border: 3px solid #939699;
  color: white;
  padding: 10px 10px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}
.button-blue {
  background-color: blue;
}
.button-red {
  background-color: red;
}
.button-black {
  background-color: black;
}
</style>
