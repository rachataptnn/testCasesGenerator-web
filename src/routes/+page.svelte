<!-- src/routes/TestCaseGenerator.svelte -->
<style>
    #editor-container {
      background-color: #272822; /* Background color */
      color: #F8F8F2; /* Text color */
      /* Add other styling properties to match the theme */
    }
</style>
<script>
    let url = '';
    let responseData = '';
    let code = '';
  
    async function handleSubmit() {
      if (url) {
        const payload = JSON.stringify({ "url": url });
        try {
          const apiResponse = await fetch("http://localhost:8080/gen-testcases", {
            method: "POST",
            body: payload,
            headers: {
              "Content-Type": "application/json"
            }
          });
          const data = await apiResponse.json();
          responseData = JSON.stringify(data, null, 2);
          code = data.testCases;
        } catch (error) {
          responseData = "Error: " + error;
        }
      }
    }
  </script>
  
  <h1>API Request Form</h1>
  
  <form on:submit={handleSubmit}>
    <label for="urlInput">URL:</label>
    <input type="text" id="urlInput" bind:value={url} placeholder="Enter URL" />
    <button type="submit">Submit</button>
  </form>
  
  <div id="response">
    <pre>{responseData}</pre>
  </div>
  
  <div id="editor-container">
    <pre>{code}</pre>
  </div>