# MicrosoftVisionAPI_C-
Accessing Microsoft Vision API via C#

Extracting text / information from images. This routine gets OCR from an Image. You need an Azure Cognitive Services Resource allocated in your Azure Portal.

using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;

ApiKeyServiceClientCredentials MicrosoftCredentials = new ApiKeyServiceClientCredentials("{YOUR ACCESS KEY}");        
string Endpoint = "https://eastus.api.cognitive.microsoft.com/";


public async Task<OCRRESULTS> OCRMicrosoftVisionAsync(string imageurl)
        {           
  
            OcrResult analysisResult = new OCRRESULTS();

            try
            {
                using (var client = new WebClient())

                    try
                    {                        
                        using (Stream stream = client.OpenRead(imageurl))
                        {                                                          
                                    using (var visionclient = new ComputerVisionClient(MicrosoftCredentials) { Endpoint = Endpoint })
                                    {                                               
                                    analysisResult = await visionclient.RecognizePrintedTextInStreamAsync(true, stream);  
                                    } 
                        }
                    }
                    catch (Exception ex)
                    {                        
                        analysisResult.Orientation = "Error " + ex.InnerException;
                    }
                        
            }
            catch (Exception ex)
            {
                analysisResult.Orientation = "Error " + ex.InnerException;                
            }

            return analysisResult;
        }
