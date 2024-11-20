# AzureOpenAI(or OpenAI) RealTime Console App with Live Audio Input and Output

For more information see this information: 

openai-dotnet: the official OpenAI library for .NET

https://www.youtube.com/watch?v=MkDJAl57reo

https://github.com/Azure-Samples/aoai-realtime-audio-sdk/tree/main/dotnet

This short console application demonstrates an interactive experience using the **NAudio** library (https://github.com/naudio/NAudio) for input and output from the default **microphone** and **speaker**

## 1. (Option 1)


## 1. (Option 2)


## 2. We create a C# Console Application with Visual Studio 2022



## 3. We load the Nuget packages


## 4. We create the 



## 5. We create the 


## 6. We configure the middleware(Program.cs)



## 7. We run the application and verify the results






## 8. Additional information

This sample uses two rudimentary multimedia abstractions built atop the **NAudio** library:

- **MicrophoneAudioStream**, which presents **pcm16** (24 KHz, 16-bit mono PCM) audio from the system default capture device as a **Stream**

- **SpeakerOutput**, which provides simple **play** and **clear** abstractions for output of **pcm16** audio to the system default render device

These multimedia abstractions are minimal stand-ins for robust audio handling and are not designed for production use

**The application workflow**:

- Configures a client from environment variables and connects a new session

- Configures the session to enable input audio transcription and provide a simplistic "I'm finished" tool, which lets the model decide when the user appears to want to finish a conversation

- Starts playback to the default output device

- Begins processing received commands from the session:

  - Upon the session successfully starting, microphone input begins

  - Upon the start of user speech being detected, any active audio output is aborted and cleared

  - When user audio input transcription is finished, the transcript is printed as feedback

  - Incremental transcript and audio data are immediately printed or rendered to the speaker

  - When an output item finishes, it's checked to see if it invoked the custom "I'm finished" tool -- if that's true, the loop ends

  - If an error is received, it's printed and the loop ends
