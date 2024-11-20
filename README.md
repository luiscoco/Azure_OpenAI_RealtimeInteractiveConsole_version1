# Azure OpenAI GPT-4o Realtime AI model (gpt-4o-realtime-preview)

For **more information** visit this site: https://github.com/Azure-Samples/aoai-realtime-audio-sdk 

Also you can see this **youtube video**: openai-dotnet: https://www.youtube.com/watch?v=MkDJAl57reo (the official OpenAI library for .NET)

And see this **application sample** in this github repo: https://github.com/Azure-Samples/aoai-realtime-audio-sdk/tree/main/dotnet

This sample also includes **functions calls**: https://github.com/robch/openai-realtime-chat-with-functions-cs

## 1. Introduction

This short console application demonstrates an interactive experience using the **NAudio** library (https://github.com/naudio/NAudio) for input and output from the default **microphone** and **speaker**


## 1. (Option 1) We get the AzureOpenAI EndPoint and Key

We login in Azure Portal and create an Azure OpenAI Service and then create a new Deployment (gpt-4o-realtime-preview)

I will explain you in more detail the steps to follow

First we create the Azure OpenAI service

Then we enter the ResourceGroup, servicename, location, service tier, and create the service

Then we navigate to manage deployments and we create a new ai model (gpt-4o-realtime-preview) deployment 

After the deployment is completed we are provide the EndPoint URL and the Key. We copy both to include in our C# Console Application



## 1. (Option 2) We get the OpenAI Key



## 2. We create a C# Console Application with Visual Studio 2022



## 3. We load the Nuget packages


## 4. We create the MicrophoneAudioStream file

The class, **MicrophoneAudioStream**, acts as a custom **Audio Stream Capturing Live Audio** data from a **microphone** using the **NAudio** library

### 4.1. Audio Stream Basics 

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
### 4.2. Audio Buffer

A **circular buffer** (_buffer) holds raw audio data. Its size is set to **store up to 10 seconds of audio**

**_bufferReadPos** and **_bufferWritePos** manage the **read/write pointers** within the buffer

**_bufferLock** ensures **thread safety** when accessing or modifying the buffer

```csharp
private readonly byte[] _buffer = new byte[BYTES_PER_SAMPLE * SAMPLES_PER_SECOND * CHANNELS * 10];
private readonly object _bufferLock = new();
private int _bufferReadPos = 0;
private int _bufferWritePos = 0;
```

### 4.3. WaveInEvent for Audio Capture

The class uses **WaveInEvent** from **NAudio** to **capture audio** data from a microphone

**WaveInEvent** is configured with the specified sample rate, bit depth, and channels

When new audio data becomes available (**DataAvailable** event), the data is written to _buffer

If the **buffer is full**, older **data is overwritten** (wrap-around behavior)

## 4.4. Setting up and Starting the Microphone Audio Stream

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

We can review the whole code:

**MicrophoneAudioStream.cs**

```csharp
using NAudio.Wave;

#nullable disable

public class MicrophoneAudioStream : Stream, IDisposable
{
    private const int SAMPLES_PER_SECOND = 24000;
    private const int BYTES_PER_SAMPLE = 2;
    private const int CHANNELS = 1;

    private readonly byte[] _buffer = new byte[BYTES_PER_SAMPLE * SAMPLES_PER_SECOND * CHANNELS * 10];
    private readonly object _bufferLock = new();
    private int _bufferReadPos = 0;
    private int _bufferWritePos = 0;

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

    public override bool CanRead => true;

    public override bool CanSeek => false;

    public override bool CanWrite => false;

    public override long Length => throw new NotImplementedException();

    public override long Position { get => throw new NotImplementedException(); set => throw new NotImplementedException(); }

    public override void Flush()
    {
        throw new NotImplementedException();
    }

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

    public override long Seek(long offset, SeekOrigin origin)
    {
        throw new NotImplementedException();
    }

    public override void SetLength(long value)
    {
        throw new NotImplementedException();
    }

    public override void Write(byte[] buffer, int offset, int count)
    {
        throw new NotImplementedException();
    }

    protected override void Dispose(bool disposing)
    {
        _waveInEvent?.Dispose();
        base.Dispose(disposing);
    }
}
```

## 4.5. Properties and methods required by the Stream base class

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

## 4.6. 


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
