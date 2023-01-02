<script>
  import { Configuration, OpenAIApi } from "openai";
  
  let maxTokens = 100;
  let isLoading = false;
  let chatbotResponse = '';
  let inputValue = '';

  const configuration = new Configuration({
    apiKey: import.meta.env.VITE_OPENAI_API_KEY
  });
  const openai = new OpenAIApi(configuration);
  
  const getResponse = async (message) => {
    try {
      isLoading = true;
      const response = await openai.createCompletion({
        model: "text-davinci-003",
        prompt: message,
        max_tokens: maxTokens,
        temperature: 0,
      });
      chatbotResponse = response.data.choices[0].text.replace('\n\n', '');
    } catch (e) {
      console.error(e.message);
      chatbotResponse = "";
    } finally {
      isLoading = false;
    }
  }
  
  const keyDownHandler = (e) => {
    if(e.key === 'Enter') {
      getResponse(inputValue);
    }
  }
  </script>
  
  <main class="m-4 p-4">
    <div class="flex flex-row justify-between w-full">
      <h1 class="text-3xl font-bold">Ask ChatGPT</h1>
      <select id="max_tokens" class="bg-grey-500" bind:value={maxTokens}>
        <option value={100} selected>Max 100 tokens</option>
        <option value={200}>Max 200 tokens</option>
        <option value={300}>Max 300 tokens</option>
        <option value={400}>Max 400 tokens</option>
        <option value={500}>Max 500 tokens</option>
      </select>
    </div>
    <div class="flex gap-4 h-fit my-4">
      <input class="bg-white rounded p-2 w-full text-black" type="text" placeholder="Write your question here" bind:value={inputValue} on:keydown={keyDownHandler}>
    </div>
    <div class="bg-black rounded my-4 p-4"><code style="white-space: pre-line;">{chatbotResponse}</code></div>
  </main>