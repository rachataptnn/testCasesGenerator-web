<style>
    #editor-container {
      background-color: #272822;
      color: #F8F8F2;
      padding: 20px;
      border-radius: 0.5rem;
    }
    
    input {
      font-family: 'Nunito', sans-serif;
      width: 100%; /* Make the input extend to 100% of its container */
      padding: 10px; /* Add some padding for spacing */
      border: 1px solid #ccc; /* Add a border with a light gray color */
      border-radius: 5px; /* Add rounded corners */
      font-size: 18px; /* Set the font size */
      outline: none; /* Remove the default input focus outline */
    }

    /* Add a hover effect for the input */
    input:hover {
        border-color: #007BFF; /* Change the border color on hover */
    }

    /* Add a focus effect for the input */
    input:focus {
        border-color: #007BFF; /* Change the border color when focused */
        box-shadow: 0 0 5px #007BFF; /* Add a subtle box shadow when focused */
    }

    button {
      font-family: 'Nunito', sans-serif;
      font-size: 18px;
      width: 100%;
      background-color: #007BFF;
      color: white;
      border: none; 
      padding: 10px 20px; 
      border-radius: 5px; 
      cursor: pointer; 
    }

    button:hover {
        background-color: rgb(8, 114, 234)
    }

    h1 {
      font-family: 'Nunito', sans-serif;
    }
    label {
      font-size: 21px;
      font-family: 'Nunito', sans-serif;
    }
    img {
      height: 1rem;
    }
    p {
      font-family: 'Nunito', sans-serif;
      font-size: 14px;
    }
    a {
      font-size: 14px;
    }
    h4 {
      font-family: 'Nunito', sans-serif;
    }
    .smallp{
      font-size: 11px;
      margin-left: 600px;
    }
    .container {
      display: flex;
    }

    .problem-set {
      margin-right: 40px;
    }
    img:hover {
      cursor: not-allowed;
    }
</style>
<script lang="ts">
  let url = '';
  let responseData = '';
  let code = '';
  let isLoading = false; // Add this variable

  async function handleSubmit() {
      isLoading = true; // Set loading state to true
      if (url) {
          const payload = JSON.stringify({ "url": url });
          try {
              const apiResponse = await fetch("https://testcasegen-prd-1315033e0e3c.herokuapp.com/gen-testcases", {
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
          } finally {
              isLoading = false; // Set loading state to false when the response is received
          }
      }
  }

  function copyTextToClipboard(text: string) { // Explicitly specify 'text' as a string
        const textToCopy = text;

        const textArea = document.createElement("textarea");
        textArea.value = textToCopy;
        document.body.appendChild(textArea);
        textArea.select();
        document.execCommand('copy');
        document.body.removeChild(textArea);

        alert("URL copied! - " + textToCopy);
    }
</script>
  
  <h1>Test cases Generator</h1>
  <p>&nbsp;&nbsp;&nbsp;&nbsp;this is a little helping tool in case of someone wanna make some go testing script but... it's not support array type yet</p>
  <p class="smallp">(actualy it's not support many)（＊´ω｀）ゞ</p>
  
  
  <div class="container">
    <div class="problem-set">
      <p class="someprb">some problems that work fine</p>
      <div>
        <a href="https://leetcode.com/problems/palindrome-number/" target="_blank">Palindrom</a>
        &nbsp;<img src="/copy.png" on:click={() => copyTextToClipboard("https://leetcode.com/problems/palindrome-number/")} alt="Click to Copy URL">
      </div>
      <div>
        <a href="https://leetcode.com/problems/guess-number-higher-or-lower/" target="_blank">guess-number-higher-or-lower</a>
        &nbsp;<img src="/copy.png" on:click={() => copyTextToClipboard("https://leetcode.com/problems/guess-number-higher-or-lower/")} alt="Click to Copy URL">
      </div>
    </div>
  
    <div class="problem-set">
      <p class="someprb">some problems that have some problem</p>
      <div>
        <a href="https://leetcode.com/problems/two-sum/" target="_blank">two-sum (not support array ;-;)</a>
        &nbsp;<img src="/copy.png" on:click={() => copyTextToClipboard("https://leetcode.com/problems/two-sum/")} alt="Click to Copy URL">
      </div>
      <div>
        <a href="https://leetcode.com/problems/roman-to-integer/" target="_blank">roman-to-integer (not support this problem too)</a>
        &nbsp;<img src="/copy.png" on:click={() => copyTextToClipboard("https://leetcode.com/problems/roman-to-integer/")} alt="Click to Copy URL">
      </div>
    </div>
  </div>

  <br/><br/>
  <form on:submit={handleSubmit}>
    <label for="urlInput">URL:</label>
    <p>&nbsp;&nbsp;How??&nbsp;&nbsp;&nbsp;&nbsp;1.copy url&nbsp;&nbsp;2.paste&nbsp;&nbsp;3.submit</p>  
    <input type="text" id="urlInput" bind:value={url} placeholder="Enter URL" />
    <button type="submit" disabled={isLoading}>Submit</button>
  </form>
  
  <br/><br/>
  <label for="urlInput">Result</label>
  <p>&nbsp;&nbsp;in term of response is quite slow just ignore it and do not wait :DD</p>
  <div id="editor-container">
    <pre>{code}</pre>
  </div>
  