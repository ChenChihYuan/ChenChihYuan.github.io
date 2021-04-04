---
title: EventAndDelegates
date: 2020-06-16 15:31:13
tags:
- Event
- Delegate
- Csharp
---

[Resource](https://www.youtube.com/watch?v=jQgwEsJISy0)

# Events
- A mechanism for communication between objects
- Used in building `Loosely Coupled Applications`
- Helps extending applications

## Example
```csharp
public class VideoEncoder{
    public void Encode(Video video){
        // Encoding logic
        // ...

        _mailService.Send(new Mail());

    }
}
```

but, in the future, if we want to send **text** message to the video owner.
```csharp
public class VideoEncoder{
    public void Encode(Video video){
        // Encoding logic
        // ...

        _mailService.Send(new Mail());
        
        _messageService.Send(new Text());
    }
}
```

but if we do it this way, the whole **Encode() method** will need to be recompiled, and the class depends on this function, will need to be recompiled and redeploied.


> We want to design our application such that when you want to change that, it changes the minimum impact on the application

We can use `Event` to deal with this problem.

# How we do event in this case

- VideoEncoder
  1. publisher
  2. event sender
- MailService
  1. Subscriber
  2. event receiver

So, in future.
- MessageService
  1. Subscriber
  2. event receiver 

## Main changes
```csharp
public class VideoEncoder{
    public void Encode(Video video){
        // Encoding logic
        // ...

        OnVideoEncoded();
    }
}
```

`OnVidoEncoded()` method simply notify the subscribers.

> **VideoEncoder** knows absoulately nothing about the **MailService** and **MessageService** or any other subscribers.
They(subscribers) can come later, and we can add more subscribers.

## One Question Raise: How VideoEncoder(publisher) notify its subscribers
We need an agreement or contract between publisher and its subscribers.

```csharp
public void OnVideoEncoded(object source, EventArgs e){

}
```

This is a very typical implementation of method in the `subscribers`, and it is called `EventHandler`.
This is also the method which called by the **publisher** when the event is raised.

Again, VideoEncoder(**Event Publisher**) do not know the existence of its publishers. Publisher **decoupled** from them(subscribers).
All the **publisher** knows, is a method(**EventHandler**).

## How do you tell VideoEncoder(Publisher) what method to call?
That's where a `Delegate` comes in.

# Delegates
- Agreement/ Contract between Publisher and Subscriber
- Determines the **signature** of the event handler method in Subscriber

1. Define a **delegate**
2. Define an event based on that delegate
3. Raise the event

```csharp
public delegate void VideoEncodedEventHandler(object source, EventArgs args);
```
- `object source` : source of that event
- `EventArgs args`: any additional data we want to send to the event


Here we declare an **event**, which hold a **reference to a function**, look like 
```csharp
void VideoEncodedEventHandler(object source, EventArgs args);
```

# Implementation

## Program.cs
```cs
namespace EventsAndDelegates
{
    class Program
    {
        static void Main(string[] args)
        {
            var video = new Video() { Title = "Video 1" };
            var videoEncoder = new VideoEncoder();          // publisher
            var mailService = new MailService();            // subscriber
            var messageSerive = new MessageService();       // subscriber


            videoEncoder.VideoEncoded += mailService.OnVideoEncoded;  // VideoEncoded event behind the scene, is a list of function pointers. OnVideoEncoded is the function pointer
            videoEncoder.VideoEncoded += messageSerive.OnVideoEncoded;


            videoEncoder.Encode(video);
        }
    }
}
```
## Video.cs
```cs
namespace EventsAndDelegates
{
    public class Video
    {
        public string Title { get; set; }
    }
}
```
## VideoEncoder.cs
```cs
using System;
using System.Threading;

namespace EventsAndDelegates
{
    public class VideoEventArgs : EventArgs
    {
        public Video Video { get; set; }
    }

    public class VideoEncoder
    {
        // 1- Define a delegate
        // 2- Define an event based on that delegate
        // 3- Raise the event

        public delegate void VideoEncodedEventHandler(object source, VideoEventArgs args);      // 1. define a delegate
        public event VideoEncodedEventHandler VideoEncoded;                                     // 2. define an event based on the delegate

        public event EventHandler<VideoEventArgs> VideoEncoding;
        public void Encode(Video video)
        {
            Console.WriteLine("Encoding Video...");
            Thread.Sleep(3000);

            OnVideoEncoded(video);                                                              // 3. Raise the event
        }
        protected virtual void OnVideoEncoded(Video video)
        {
            if (VideoEncoded != null)
                VideoEncoded(this, new VideoEventArgs() { Video = video });
        }
    }
}
```

## MailService.cs
```cs
using System;

namespace EventsAndDelegates
{
    public class MailService
    {
        public void OnVideoEncoded(object source, VideoEventArgs e)
        {
            Console.WriteLine("MailService: Sending an email..." + e.Video.Title);
        }
    }
}
```

## MessageService.cs
```cs
using System;

namespace EventsAndDelegates
{
    public class MessageService
    {
        public void OnVideoEncoded(object source, VideoEventArgs e)
        {
            Console.WriteLine("MessageService: Sending a text message..." + e.Video.Title);
        }
    }
}
```

## Result
![idea](/uploads/Delegate1.JPG)
