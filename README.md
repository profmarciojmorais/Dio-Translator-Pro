# Dio-Translator-Pro

Processo de ativação dos serviços de tradução do Azure AI
Abaixo um exemplo do código de traduçao de um trecho de uma musica usando um Translator:
Inglês

Just The Way You Are
Bruno Mars

Oh, her eyes, her eyes
Make the stars look like they're not shinin'
Her hair, her hair
Falls perfectly without her trying

Português(Brasil)
Do jeito que você é

Oh, seus olhos, seus olhos
Faça as estrelas parecerem que não estão brilhando
Seu cabelo, seu cabelo
Cai perfeitamente sem ela tentar

View Request
[{
    "text": "Just The Way You Are

Oh, her eyes, her eyes
Make the stars look like they're not shinin'
Her hair, her hair
Falls perfectly without her trying"
}]

View response
[{
    "detectedLanguage": {
        "language": "en",
        "score": "1"
    }
    "translations": [{
        "text": "Do jeito que você é

Oh, seus olhos, seus olhos
Faça as estrelas parecerem que não estão brilhando
Seu cabelo, seu cabelo
Cai perfeitamente sem ela tentar",
        "to": "pt"
    }]
}]

Code Simple

using System.Text;
using Newtonsoft.Json;

class Program
{
    private static readonly string key = "<your-translator-key>";
    private static readonly string endpoint = "https://api.cognitive.microsofttranslator.com";

    // location, also known as region.
    // required if you're using a multi-service or regional (not global) resource. It can be found in the Azure portal on the Keys and Endpoint page.
    private static readonly string location = "<YOUR-RESOURCE-LOCATION>";

    static async Task Main(string[] args)
    {
        // Input and output languages are defined as parameters.
        string route = "/translate?api-version=3.0&from=en&to=fr&to=zu";
        string textToTranslate = "I would really like to drive your car around the block a few times!";
        object[] body = new object[] { new { Text = textToTranslate } };
        var requestBody = JsonConvert.SerializeObject(body);

        using (var client = new HttpClient())
        using (var request = new HttpRequestMessage())
        {
            // Build the request.
            request.Method = HttpMethod.Post;
            request.RequestUri = new Uri(endpoint + route);
            request.Content = new StringContent(requestBody, Encoding.UTF8 "application/json");
            request.Headers.Add("Ocp-Apim-Subscription-Key", key);
            // location required if you're using a multi-service or regional (not global) resource.
            request.Headers.Add("Ocp-Apim-Subscription-Region", location);

            // Send the request and get response.
            HttpResponseMessage response = await client.SendAsync(request).ConfigureAwait(false);
            // Read response as a string.
            string result = await response.Content.ReadAsStringAsync();
            Console.WriteLine(result);
        }
    }
}
