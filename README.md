# Azure OpenAI GPT-4o Realtime AI model (gpt-4o-realtime-preview)

For **more information** visit this site: https://github.com/Azure-Samples/aoai-realtime-audio-sdk 

Also you can see this **youtube video**: openai-dotnet: https://www.youtube.com/watch?v=MkDJAl57reo (the official OpenAI library for .NET)

And see this **application sample** in this github repo: https://github.com/Azure-Samples/aoai-realtime-audio-sdk/tree/main/dotnet

This sample also includes **functions calls**: https://github.com/robch/openai-realtime-chat-with-functions-cs

## 1. Introduction

This application is a **real-time audio-based conversational system** using **Azure OpenAI** services

It connects to the Azure OpenAI platform and allows users to have voice conversations with an AI model in real-time

This short console application demonstrates an interactive experience using the **NAudio** library (https://github.com/naudio/NAudio) for input and output from the default **microphone** and **speaker**

This application acts as a **real-time**, **voice-enabled AI assistant** powered by **Azure OpenAI**

It integrates voice input and output seamlessly, providing an interactive and hands-free user experience

The event-driven architecture ensures low latency and responsiveness, making it suitable for conversational interfaces, customer support bots, or accessibility tools.



## 2. (Option 1) We get the AzureOpenAI EndPoint and Key

We login in Azure Portal and create an Azure OpenAI Service and then create a new Deployment (gpt-4o-realtime-preview)

I will explain you in more detail the steps to follow

First we create the Azure OpenAI service

Then we enter the ResourceGroup, servicename, location, service tier, and create the service

Then we navigate to manage deployments and we create a new ai model (gpt-4o-realtime-preview) deployment 

After the deployment is completed we are provide the EndPoint URL and the Key. We copy both to include in our C# Console Application

We login in Azure Portal and select the Azure OpenAI service

![image](https://github.com/user-attachments/assets/0d6b477a-1b8b-4426-ab69-0e12971ddc63)

We press in the Create button

![image](https://github.com/user-attachments/assets/04102886-7a19-4f83-b3db-b27f035b4cfd)

We input the Azure Open AI service definition data and press the Next button and finally the Create button

![image](https://github.com/user-attachments/assets/88356a49-7940-4686-a08e-f2df0d70ec92)

![image](https://github.com/user-attachments/assets/f5cd1878-5572-480e-bd2e-9ee09094f0f1)

![image](https://github.com/user-attachments/assets/f7ded4ff-8a04-49a1-934e-c11b6e14fe4c)

![image](https://github.com/user-attachments/assets/d5d60d9d-543c-4ad7-8b10-c44ef7fabd3b)

We verify the service was created

![image](https://github.com/user-attachments/assets/9668483e-d43e-42b5-b6fc-eb5bbd26d96f)

![image](https://github.com/user-attachments/assets/3a523e85-5ce4-4c0f-8e8c-55a735c3e076)

Now we create a AI model deployment

![image](https://github.com/user-attachments/assets/cfb50ba4-27b7-4b49-b5db-21b3c9ff947d)

![image](https://github.com/user-attachments/assets/4a7ff54f-fa43-4cba-b4fd-8a68270243b8)

![image](https://github.com/user-attachments/assets/66079e08-c384-4f73-a31c-9b84f698311d)

![image](https://github.com/user-attachments/assets/10da5431-7c9e-4fbd-89d2-11a5524100b3)

**IMPORTANT NOTE**: confirm the Region to deploy the AI model is the same as you chose when creating the Azure OpenAI service

We copy the Endpoint URL and API Key for using them in the C# Console Application

![image](https://github.com/user-attachments/assets/62dcd5e0-4a04-46e3-a71d-4c9dbcfde3b4)


## 2. (Option 2) We get the OpenAI Key

### 2.1. Sign up or Log in to the OpenAI platform

In the internet browser we navigate to the OpenAI platform URL: **https://platform.openai.com/**

We press on the **Sign up** button

![image](https://github.com/luiscoco/ChatGPT_OpenAI-Sample1-QuickStart/assets/32194879/b634a849-300f-4cc0-82b8-11a6e6135e50)

We **create a new account** or if we already have one we **log in** to our account

![image](https://github.com/luiscoco/ChatGPT_OpenAI-Sample1-QuickStart/assets/32194879/193d0d27-0396-4fdf-8ba2-ed614d1b7f33)

### 2.2. Account settings (set the payment method and spend limit)

We select the **Settings->Billing** option in the left hand side menu

https://platform.openai.com/account/billing/overview

![image](https://github.com/luiscoco/ChatGPT_OpenAI-Sample1-QuickStart/assets/32194879/a7b275e9-14f9-4fb3-a1d1-b284bc3dae67)

We have to enter the **Payment method**

https://platform.openai.com/account/billing/payment-methods

![image](https://github.com/luiscoco/ChatGPT_OpenAI-Sample1-QuickStart/assets/32194879/ef9128b8-c110-4648-8e92-b36175b43df0)

We input all the info required in the payment method form

![image](https://github.com/luiscoco/ChatGPT_OpenAI-Sample1-QuickStart/assets/32194879/98830a0f-0148-483f-acc9-51c35beb405b)

We can set an initial ammout of 12 euros per month

https://platform.openai.com/account/limits

![image](https://github.com/luiscoco/ChatGPT_OpenAI-Sample1-QuickStart/assets/32194879/d1418bb6-2e7a-41c9-a745-1b898fa90380)

### 2.3. Create API Key

We have to create an API Key in order to authorize when we send the API request

![image](https://github.com/luiscoco/ChatGPT_OpenAI-Sample1-QuickStart/assets/32194879/ac69918b-d490-4e67-8486-7f36a8a66a5b)

We copy and store the API Key value, we will need to introduce this value in the API header to authorize the service

## 3. We create a C# Console Application with Visual Studio 2022



## 4. We load the Nuget packages


## 5. We create the MicrophoneAudioStream file

The class, **MicrophoneAudioStream**, acts as a custom **Audio Stream Capturing Live Audio** data from a **microphone** using the **NAudio** library

### 5.1. Audio Stream Basics 

The class **MicrophoneAudioStream** extends **Stream**, providing custom implementations for reading audio data. It also implements **IDisposable** to manage resources

It defines **constants** for **audio configuration**:

**SAMPLES_PER_SECOND**: The sample rate of 24000 Hz

**BYTES_PER_SAMPLE**: Each audio sample is 2 bytes (16-bit audio)

**CHANNELS**: Mono audio (1 channel)

```csharp
private const int SAMPLES_PER_SECOND = 24000;
private const int BYTES_PER_SAMPLE = 2;
private const int CHANNELS = 1;
```
### 5.2. Audio Buffer

A **circular buffer** (_buffer) holds raw audio data. Its size is set to **store up to 10 seconds of audio**

**_bufferReadPos** and **_bufferWritePos** manage the **read/write pointers** within the buffer

**_bufferLock** ensures **thread safety** when accessing or modifying the buffer

```csharp
private readonly byte[] _buffer = new byte[BYTES_PER_SAMPLE * SAMPLES_PER_SECOND * CHANNELS * 10];
private readonly object _bufferLock = new();
private int _bufferReadPos = 0;
private int _bufferWritePos = 0;
```

### 5.3. WaveInEvent for Audio Capture

The class uses **WaveInEvent** from **NAudio** to **capture audio** data from a microphone

**WaveInEvent** is configured with the specified sample rate, bit depth, and channels

When new audio data becomes available (**DataAvailable** event), the data is written to _buffer

If the **buffer is full**, older **data is overwritten** (wrap-around behavior)

## 5.4. Setting up and Starting the Microphone Audio Stream

This snippet is part of the **MicrophoneAudioStream class** and is responsible for **setting up and starting the microphone audio stream** using the **NAudio** library

**Field Definition**:

```csharp
private readonly WaveInEvent _waveInEvent;
```

As we mentioned before **WaveInEvent** is an NAudio component that captures audio from the microphone asynchronously

**Constructor**:

```csharp
private MicrophoneAudioStream()
```
The constructor **initializes** the **_waveInEvent** and **configures** its properties and behavior for capturing audio

**Wave Format Setup**:

```csharp
Copy code
_waveInEvent = new()
{
    WaveFormat = new WaveFormat(SAMPLES_PER_SECOND, BYTES_PER_SAMPLE * 8, CHANNELS),
};
```

The **WaveFormat** specifies the audio format for the capture:

SAMPLES_PER_SECOND: Sampling rate (e.g., 24000 Hz)

BYTES_PER_SAMPLE * 8: Converts bytes to bits (e.g., 16 bits for 2 bytes)

CHANNELS: Mono (1 channel)

**Handling Captured Audio**:

```csharp
_waveInEvent.DataAvailable += (_, e) =>
{
    lock (_bufferLock)
    {
        ...
    }
};
```

The **DataAvailable event** triggers whenever new audio data is available from the microphone

Inside the event handler:

The incoming audio data (**e.Buffer**) is written to a **circular buffer** (**_buffer**)

If the write position reaches the end of the buffer, it wraps around to the beginning

**Circular Buffer Logic**:

```csharp
if (_bufferWritePos + bytesToCopy >= _buffer.Length)
{
    int bytesToCopyBeforeWrap = _buffer.Length - _bufferWritePos;
    Array.Copy(e.Buffer, 0, _buffer, _bufferWritePos, bytesToCopyBeforeWrap);
    bytesToCopy -= bytesToCopyBeforeWrap;
    _bufferWritePos = 0;
}
Array.Copy(e.Buffer, e.BytesRecorded - bytesToCopy, _buffer, _bufferWritePos, bytesToCopy);
_bufferWritePos += bytesToCopy;
```

Handles the "**wrap-around**" scenario for the circular buffer:

Part of the incoming data is written at the end of the buffer

Remaining data starts writing from the beginning of the buffer

**Start Recording**:

```csharp
_waveInEvent.StartRecording();
```

**Begins capturing audio** from the microphone

**Factory Method**:

```csharp
public static MicrophoneAudioStream Start() => new();
```

A static method that simplifies creating and starting the **MicrophoneAudioStream**

We can also review the whole code in this section:

```csharp
 private readonly WaveInEvent _waveInEvent;

 private MicrophoneAudioStream()
 {
     _waveInEvent = new()
     {
         WaveFormat = new WaveFormat(SAMPLES_PER_SECOND, BYTES_PER_SAMPLE * 8, CHANNELS),
     };
     _waveInEvent.DataAvailable += (_, e) =>
     {
         lock (_bufferLock)
         {
             int bytesToCopy = e.BytesRecorded;
             if (_bufferWritePos + bytesToCopy >= _buffer.Length)
             {
                 int bytesToCopyBeforeWrap = _buffer.Length - _bufferWritePos;
                 Array.Copy(e.Buffer, 0, _buffer, _bufferWritePos, bytesToCopyBeforeWrap);
                 bytesToCopy -= bytesToCopyBeforeWrap;
                 _bufferWritePos = 0;
             }
             Array.Copy(e.Buffer, e.BytesRecorded - bytesToCopy, _buffer, _bufferWritePos, bytesToCopy);
             _bufferWritePos += bytesToCopy;
         }
     };
     _waveInEvent.StartRecording();
 }

 public static MicrophoneAudioStream Start() => new();
```

## 5.5. Properties and methods required by the Stream base class

This code defines how the **MicrophoneAudioStream** interacts with the **Stream API**:

It enables **read-only** functionality while disabling seeking and writing

**Unimplemented methods** (Length, Position, Flush) are irrelevant for a real-time audio stream and throw exceptions if called

**CanRead**:

```csharp
public override bool CanRead => true;
```

Indicates that the stream supports reading operations

Always returns true because MicrophoneAudioStream is designed to read audio data

**CanSeek**:

```csharp
public override bool CanSeek => false;
```

Indicates that the stream does not support seeking (moving the position within the stream)

Always returns false, as seeking is not applicable for a real-time audio stream

**CanWrite**:

```csharp
public override bool CanWrite => false;
```

Indicates that the stream does not support writing data

Always returns false, as this is a read-only stream for microphone input

**Length**:

```csharp
public override long Length => throw new NotImplementedException();
```

Represents the total length of the stream

Throws a NotImplementedException because the concept of "length" does not apply to a live audio stream

**Position**:

```csharp
public override long Position { get => throw new NotImplementedException(); set => throw new NotImplementedException(); }
```

Represents the current position within the stream

Throws a NotImplementedException because position tracking is irrelevant for a continuous, real-time stream

**Flush**:

```csharp
public override void Flush()
{
    throw new NotImplementedException();
}
```

Clears any buffered data in the stream

Throws a NotImplementedException because there is no concept of flushing in this implementation

## 5.6. Reading the Audio Data

This code defines the **Read** method, which **reads audio data from a circular buffer** (_buffer) into a user-provided buffer

This method is essential for streaming applications where **audio data is continuously captured** into a circular buffer and needs to be processed or **consumed in chunks** by other parts of the system

The method retrieves a specified number of bytes (count) of audio data from the circular buffer, handling wrap-around if needed, and ensures thread safety during the read operation

**Key Behaviors**:

Blocking Read: If insufficient data is available, the method waits until enough data is present in the buffer

Thread Safety: Uses a lock to prevent concurrent modifications to the buffer during read operations

Circular Buffer Management: Efficiently handles cases where the read request spans the end and beginning of the circular buffer

```csharp
public override int Read(byte[] buffer, int offset, int count)
{
    int totalCount = count;

    int GetBytesAvailable() => _bufferWritePos < _bufferReadPos
        ? _bufferWritePos + (_buffer.Length - _bufferReadPos)
        : _bufferWritePos - _bufferReadPos;

    while (GetBytesAvailable() < count)
    {
        Thread.Sleep(100);
    }

    lock (_bufferLock)
    {
        if (_bufferReadPos + count >= _buffer.Length)
        {
            int bytesBeforeWrap = _buffer.Length - _bufferReadPos;
            Array.Copy(_buffer, _bufferReadPos, buffer, offset, bytesBeforeWrap);
            _bufferReadPos = 0;
            count -= bytesBeforeWrap;
            offset += bytesBeforeWrap;
        }

        Array.Copy(_buffer, _bufferReadPos, buffer, offset, count);
        _bufferReadPos += count;
    }

    return totalCount;
}
```

### 5.7. Unimplemented Methods and Resource Cleanup

**Unimplemented Methods**: Methods like **Seek**, **SetLength**, and **Write** are not relevant for the stream's functionality and are deliberately left unimplemented

**Resource Cleanup**: The Dispose method ensures proper cleanup of resources, especially the WaveInEvent instance, when the object is no longer in use

The **Seek** method is used to change the current position within a stream

It is not implemented because seeking is irrelevant for a real-time audio stream

A NotImplementedException is thrown if this method is called

```csharp
public override long Seek(long offset, SeekOrigin origin)
{
    throw new NotImplementedException();
}
```

The **SetLength** method is used to set the length of a stream

It is not implemented because the length of a real-time audio stream is dynamic and cannot be predetermined or set

Throws a NotImplementedException if called

```csharp
public override void SetLength(long value)
{
    throw new NotImplementedException();
}
```

The **Write** method allows data to be written to a stream

Not implemented because the MicrophoneAudioStream is read-only, intended only for reading audio data

Throws a NotImplementedException if called

```csharp
public override void Write(byte[] buffer, int offset, int count)
{
    throw new NotImplementedException();
}
```

**Dispose** method releases resources when the stream is no longer needed

Disposes the _waveInEvent object to release the microphone resource and any unmanaged resources used by NAudio

Calls the base class Dispose method to ensure proper disposal of the parent class

```csharp
protected override void Dispose(bool disposing)
{
    _waveInEvent?.Dispose();
    base.Dispose(disposing);
}
```

## 6. We create the SpeakerOutput file

This C# code defines a class **SpeakerOutput** that uses the **NAudio** library to **play audio** through the system's speakers

The **SpeakerOutput** class facilitates audio playback by buffering and queuing audio data, then playing it using WaveOutEvent from the NAudio library

It provides methods to enqueue audio data for playback, clear the buffer, and manage resource cleanup

**Key Components**

**BufferedWaveProvider (_waveProvider)**:

Acts as a buffer to hold audio data for playback

It is configured with a specific audio format (WaveFormat) to define playback properties like sample rate, bit depth, and number of channels

**WaveOutEvent (_waveOutEvent)**:

Manages audio playback on the speaker

It initializes with the BufferedWaveProvider to continuously play the buffered audio

**Methods**

**Constructor (SpeakerOutput)**:

Sets up the audio format with a sample rate of 24,000 Hz, 16-bit depth, and mono (1 channel)

Creates a BufferedWaveProvider with a 2-minute buffer duration

Initializes the WaveOutEvent to play audio from the buffer

**EnqueueForPlayback**:

Accepts audio data (BinaryData) as input

Converts the data to a byte array and adds it to the _waveProvider buffer for playback

Handles null or empty audio data gracefully by logging a warning

**ClearPlayback**:

Clears any buffered audio data in _waveProvider, stopping current playback and removing queued audio

**Dispose**:

Cleans up resources by disposing of the WaveOutEvent object, releasing system resources used for audio playback

**Usage**

This class is useful in scenarios where you need to dynamically queue and play audio, such as real-time audio streams or audio notifications

**Example usage**:

**Instantiate** SpeakerOutput

Call **EnqueueForPlayback** with binary audio data to play it

Use **ClearPlayback** to stop playback and clear the buffer

Always call **Dispose** to release resources when done

**Practical Details**

WaveFormat: Configures the playback format (e.g., 24000 Hz mono)

Buffering: Allows queuing audio data to ensure smooth playback even if input data arrives asynchronously

Error Handling: Warns about invalid audio input but does not throw exceptions, making it robust for real-time use

This class encapsulates low-level audio management in a straightforward API, leveraging NAudio's functionality to simplify audio playback operations

We can review the whole code for playing the audio

```csharp
using NAudio.Wave;

public class SpeakerOutput : IDisposable
{
    BufferedWaveProvider _waveProvider;
    WaveOutEvent _waveOutEvent;

    public SpeakerOutput()
    {
        WaveFormat outputAudioFormat = new(
            rate: 24000,
            bits: 16,
            channels: 1);
        _waveProvider = new(outputAudioFormat)
        {
            BufferDuration = TimeSpan.FromMinutes(2),
        };
        _waveOutEvent = new();
        _waveOutEvent.Init(_waveProvider);
        _waveOutEvent.Play();
    }

    public void EnqueueForPlayback(BinaryData audioData)
    {
        if (audioData == null || audioData.ToArray().Length == 0)
        {
            Console.WriteLine("Warning: Received null or empty audio data.");
            return;
        }

        byte[] buffer = audioData.ToArray();
        _waveProvider.AddSamples(buffer, 0, buffer.Length);
    }

    public void ClearPlayback()
    {
        _waveProvider.ClearBuffer();
    }

    public void Dispose()
    {
        _waveOutEvent?.Dispose();
    }
}
```

## 7. We configure the middleware(Program.cs)

```csharp
using Azure.AI.OpenAI;
using Azure.Identity;
using OpenAI;
using OpenAI.RealtimeConversation;
using System.ClientModel;

#pragma warning disable OPENAI002

public class Program
{
    public static async Task Main(string[] args)
    {
        // First, we create a client according to configured environment variables (see end of file) and then start
        // a new conversation session.
        RealtimeConversationClient client = GetConfiguredClient();
        using RealtimeConversationSession session = await client.StartConversationSessionAsync();

        // We'll add a simple function tool that enables the model to interpret user input to figure out when it
        // might be a good time to stop the interaction.
        ConversationFunctionTool finishConversationTool = new()
        {
            Name = "user_wants_to_finish_conversation",
            Description = "Invoked when the user says goodbye, expresses being finished, or otherwise seems to want to stop the interaction.",
            Parameters = BinaryData.FromString("{}")
        };

        // Now we configure the session using the tool we created along with transcription options that enable input
        // audio transcription with whisper.
        await session.ConfigureSessionAsync(new ConversationSessionOptions()
        {
            Tools = { finishConversationTool },
            InputTranscriptionOptions = new()
            {
                Model = "whisper-1",
            },
        });

        // For convenience, we'll proactively start playback to the speakers now. Nothing will play until it's enqueued.
        SpeakerOutput speakerOutput = new();

        // With the session configured, we start processing commands received from the service.
        await foreach (ConversationUpdate update in session.ReceiveUpdatesAsync())
        {
            // session.created is the very first command on a session and lets us know that connection was successful.
            if (update is ConversationSessionStartedUpdate)
            {
                Console.WriteLine($" <<< Connected: session started");
                // This is a good time to start capturing microphone input and sending audio to the service. The
                // input stream will be chunked and sent asynchronously, so we don't need to await anything in the
                // processing loop.
                _ = Task.Run(async () =>
                {
                    using MicrophoneAudioStream microphoneInput = MicrophoneAudioStream.Start();
                    Console.WriteLine($" >>> Listening to microphone input");
                    Console.WriteLine($" >>> (Just tell the app you're done to finish)");
                    Console.WriteLine();
                    await session.SendInputAudioAsync(microphoneInput);
                });
            }

            // input_audio_buffer.speech_started tells us that the beginning of speech was detected in the input audio
            // we're sending from the microphone.
            if (update is ConversationInputSpeechStartedUpdate speechStartedUpdate)
            {
                Console.WriteLine($" <<< Start of speech detected @ {speechStartedUpdate.AudioStartTime}");
                // Like any good listener, we can use the cue that the user started speaking as a hint that the app
                // should stop talking. Note that we could also track the playback position and truncate the response
                // item so that the model doesn't "remember things it didn't say" -- that's not demonstrated here.
                speakerOutput.ClearPlayback();
            }

            // input_audio_buffer.speech_stopped tells us that the end of speech was detected in the input audio sent
            // from the microphone. It'll automatically tell the model to start generating a response to reply back.
            if (update is ConversationInputSpeechFinishedUpdate speechFinishedUpdate)
            {
                Console.WriteLine($" <<< End of speech detected @ {speechFinishedUpdate.AudioEndTime}");
            }

            // conversation.item.input_audio_transcription.completed will only arrive if input transcription was
            // configured for the session. It provides a written representation of what the user said, which can
            // provide good feedback about what the model will use to respond.
            if (update is ConversationInputTranscriptionFinishedUpdate transcriptionFinishedUpdate)
            {
                Console.WriteLine($" >>> USER: {transcriptionFinishedUpdate.Transcript}");
            }

            // Item streaming delta updates provide a combined view into incremental item data including output
            // the audio response transcript, function arguments, and audio data.
            if (update is ConversationItemStreamingPartDeltaUpdate deltaUpdate)
            {
                if (deltaUpdate.AudioBytes == null || deltaUpdate.AudioBytes.ToArray().Length == 0)
                {
                    //Console.WriteLine("Warning: Received null or empty audio bytes.");
                }
                else
                {
                    speakerOutput.EnqueueForPlayback(deltaUpdate.AudioBytes);
                }
                Console.Write(deltaUpdate.AudioTranscript);
                Console.Write(deltaUpdate.Text);
            }

            // response.output_item.done tells us that a model-generated item with streaming content is completed.
            // That's a good signal to provide a visual break and perform final evaluation of tool calls.
            if (update is ConversationItemStreamingFinishedUpdate itemFinishedUpdate)
            {
                Console.WriteLine();
                if (itemFinishedUpdate.FunctionName == finishConversationTool.Name)
                {
                    Console.WriteLine($" <<< Finish tool invoked -- ending conversation!");
                    break;
                }
            }

            // error commands, as the name implies, are raised when something goes wrong.
            if (update is ConversationErrorUpdate errorUpdate)
            {
                Console.WriteLine();
                Console.WriteLine();
                Console.WriteLine($" <<< ERROR: {errorUpdate.Message}");
                Console.WriteLine(errorUpdate.GetRawContent().ToString());
                break;
            }
        }
    }

    private static RealtimeConversationClient GetConfiguredClient()
    {
        string? aoaiEndpoint = "https://myazureopenaiserviceluxoftluis.openai.azure.com/";
        //string? aoaiEndpoint = Environment.GetEnvironmentVariable("AZURE_OPENAI_ENDPOINT");
        string? aoaiUseEntra = Environment.GetEnvironmentVariable("AZURE_OPENAI_USE_ENTRA");
        //string? aoaiDeployment = Environment.GetEnvironmentVariable("AZURE_OPENAI_DEPLOYMENT");
        string? aoaiDeployment = "gpt-4o-realtime-preview";
        //string? aoaiApiKey = Environment.GetEnvironmentVariable("AZURE_OPENAI_API_KEY");
        string? aoaiApiKey = "AZURE_OPENAI_API_KEY";
        //string? oaiApiKey = Environment.GetEnvironmentVariable("OPENAI_API_KEY");
        string? oaiApiKey = "OPENAI_API_KEY";

        if (aoaiEndpoint is not null && bool.TryParse(aoaiUseEntra, out bool useEntra) && useEntra)
        {
            return GetConfiguredClientForAzureOpenAIWithEntra(aoaiEndpoint, aoaiDeployment);
        }
        else if (aoaiEndpoint is not null && aoaiApiKey is not null)
        {
            return GetConfiguredClientForAzureOpenAIWithKey(aoaiEndpoint, aoaiDeployment, aoaiApiKey);
        }
        else if (aoaiEndpoint is not null)
        {
            throw new InvalidOperationException(
                $"AZURE_OPENAI_ENDPOINT configured without AZURE_OPENAI_USE_ENTRA=true or AZURE_OPENAI_API_KEY.");
        }
        else if (oaiApiKey is not null)
        {
            return GetConfiguredClientForOpenAIWithKey(oaiApiKey);
        }
        else
        {
            throw new InvalidOperationException(
                $"No environment configuration present. Please provide one of:\n"
                    + " - AZURE_OPENAI_ENDPOINT with AZURE_OPENAI_USE_ENTRA=true or AZURE_OPENAI_API_KEY\n"
                    + " - OPENAI_API_KEY");
        }
    }

    private static RealtimeConversationClient GetConfiguredClientForAzureOpenAIWithEntra(
        string aoaiEndpoint,
        string? aoaiDeployment)
    {
        Console.WriteLine($" * Connecting to Azure OpenAI endpoint (AZURE_OPENAI_ENDPOINT): {aoaiEndpoint}");
        Console.WriteLine($" * Using Entra token-based authentication (AZURE_OPENAI_USE_ENTRA)");
        Console.WriteLine(string.IsNullOrEmpty(aoaiDeployment)
            ? $" * Using no deployment (AZURE_OPENAI_DEPLOYMENT)"
            : $" * Using deployment (AZURE_OPENAI_DEPLOYMENT): {aoaiDeployment}");

        AzureOpenAIClient aoaiClient = new(new Uri(aoaiEndpoint), new DefaultAzureCredential());
        return aoaiClient.GetRealtimeConversationClient(aoaiDeployment);
    }

    private static RealtimeConversationClient GetConfiguredClientForAzureOpenAIWithKey(
        string aoaiEndpoint,
        string? aoaiDeployment,
        string aoaiApiKey)
    {
        Console.WriteLine($" * Connecting to Azure OpenAI endpoint (AZURE_OPENAI_ENDPOINT): {aoaiEndpoint}");
        Console.WriteLine($" * Using API key (AZURE_OPENAI_API_KEY): {aoaiApiKey[..5]}**");
        Console.WriteLine(string.IsNullOrEmpty(aoaiDeployment)
            ? $" * Using no deployment (AZURE_OPENAI_DEPLOYMENT)"
            : $" * Using deployment (AZURE_OPENAI_DEPLOYMENT): {aoaiDeployment}");

        AzureOpenAIClient aoaiClient = new(new Uri(aoaiEndpoint), new ApiKeyCredential(aoaiApiKey));
        return aoaiClient.GetRealtimeConversationClient(aoaiDeployment);
    }

    private static RealtimeConversationClient GetConfiguredClientForOpenAIWithKey(string oaiApiKey)
    {
        string oaiEndpoint = "https://api.openai.com/v1";
        Console.WriteLine($" * Connecting to OpenAI endpoint (OPENAI_ENDPOINT): {oaiEndpoint}");
        Console.WriteLine($" * Using API key (OPENAI_API_KEY): {oaiApiKey[..5]}**");

        OpenAIClient aoaiClient = new(new ApiKeyCredential(oaiApiKey));
        return aoaiClient.GetRealtimeConversationClient("gpt-4o-realtime-preview-2024-10-01");
    }
}
```

## 8. We run the application and verify the results

![image](https://github.com/user-attachments/assets/fd46a0d7-844d-4267-a481-0bff2188ef59)

When we ask the assistant to finish the conversation the **Finish tool** is invoked

![image](https://github.com/user-attachments/assets/b013b903-cd2e-4ac3-998a-54b21b972b75)

## 9. Additional information

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
