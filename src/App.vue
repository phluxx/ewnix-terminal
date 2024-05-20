<template>
  <div class="terminal">
    <div class="output">
      <div v-for="line in output" :key="line.id">{{ line }}</div>
    </div>
    <div class="prompt">
      <span>$</span>
      <input v-model="command" @keyup.enter="executeCommand" autofocus />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue';

export default defineComponent({
  setup() {
    const command = ref<string>('');
    const output = ref<string[]>([]);
    const helpMessage = [
      " __    __     _                          ",
      "/ / /\\ \\ \\___| | ___ ___  _ __ ___   ___ ",
      "\\ \\/  \\/ / _ \\ |/ __/ _ \\| '_ ` _ \\ / _ \\",
      " \\  /\\  /  __/ | (_| (_) | | | | | |  __/",
      "  \\/  \\/ \\___|_|\\___\\___/|_| |_| |_|\\___|",
      "                                         ",
      "-------------------------------------------",
      "",
      "Available commands are: about, help",
      "",
      "This is the help page."
    ];
    const aboutMessage = [
      "   _   _                 _   ",
      "  /_\\ | |__   ___  _   _| |_ ",
      " //_\\\\| '_ \\ / _ \\| | | | __|",
      "/  _  \\ |_) | (_) | |_| | |_ ",
      "\\_/ \\_/_.__/ \\___/ \\__,_|\\__|",
      "",
      "-------------------------------------------",
      "",
      "Ewnix is a Kubernetes project started by phlux.",
      "",
      "Ewnix is a versatile Kubernetes cluster set up to support a wide",
      "range of projects and services, including:",
      "",
      "- **FOSS Development**: A platform for fostering open-source",
      "  software development, enabling collaboration and innovation.",
      "",
      "- **Web Hosting**: Reliable and scalable web hosting for various",
      "  applications and websites.",
      "",
      "- **IRC Hosting**: Providing robust and secure Internet Relay",
      "  Chat (IRC) hosting for community interactions and support.",
      "",
      "Ewnix leverages Kubernetes to ensure high availability, scalability,",
      "and efficient resource management, making it an ideal solution",
      "for modern cloud-native applications.",
      "",
      "I’ll put more content here later."
    ];
    const pages = ref<Record<string, string[]>>({
      about: aboutMessage,
      help: helpMessage,
    });

    const executeCommand = () => {
      const cmd = command.value.trim().toLowerCase();
      if (pages.value[cmd]) {
        output.value = pages.value[cmd]; // Clear the output and display the new content
      } else {
        output.value = [
          `Unknown command: ${cmd}`,
          "",
          "Available commands are: about, help",
        ];
      }
      command.value = '';
      scrollToBottom();
    };

    const scrollToBottom = () => {
      const terminalOutput = document.querySelector('.output');
      if (terminalOutput) {
        terminalOutput.scrollTop = terminalOutput.scrollHeight;
      }
    };

    onMounted(() => {
      output.value.push(...helpMessage);
      scrollToBottom();
    });

    return {
      command,
      output,
      executeCommand,
    };
  },
});
</script>

<style>
body {
  background-color: #000;
  color: #0f0;
  font-family: "Courier New", Courier, monospace;
  margin: 0;
  padding: 0;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.terminal {
  display: flex;
  flex-direction: column;
  height: 100vh;
  width: 100vw;
  padding: 1rem;
  box-sizing: border-box;
}

.output {
  flex-grow: 1;
  overflow-y: auto;
  white-space: pre; /* Preserve formatting */
}

.prompt {
  display: flex;
  align-items: center;
}

.prompt span {
  margin-right: 0.5rem;
}

input {
  background: none;
  border: none;
  color: #0f0;
  font-family: inherit;
  outline: none;
  width: 100%;
}
</style>

