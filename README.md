# Text-To-Speech-UWP
Simple Text To Speech In UWP

UWP – Introduction to Text to Speech

Now in my app I want to be able to have the dealer tell me which cards have been dealt. For this, we can use the Windows.Media.SpeechSynthesis.SpeechSynthesizer class (link: https://msdn.microsoft.com/en-us/library/windows.media.speechsynthesis.speechsynthesizer.aspx). Here is a really basic example to get some speech output:

On my XAML page:

   <StackPanel>
        <TextBox Header="Text To Speak" x:Name="textToSpeechBox" />
        <Button Click="Button_Click" Content="Click Me!"/>
        <MediaElement x:Name="mediaElement" />
    </StackPanel>

This simply adds a text box and a button to the page. When you click on the button the text in the box will be spoken by the synthesizer. Here is the code behind:

    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }

        private async void Button_Click(object sender, RoutedEventArgs e)
        {
            var speechText = this.textToSpeechBox.Text;
            if (speechText != "")
            {
                var synth = new SpeechSynthesizer();

                var speechStream = await synth.SynthesizeTextToStreamAsync(speechText);

                this.mediaElement.AutoPlay = true;
                this.mediaElement.SetSource(speechStream, speechStream.ContentType);
                this.mediaElement.Play();
            }
        }
    }


On the action of the button click we take the text from the textbox, create a synthesizer, create a speech stream, then play it through the media element that we added to the page.

Changing voice?

It is possible to select a different voice to be used. This example shows how to select a male voice:

var maleVoice = SpeechSynthesizer.AllVoices.FirstOrDefault(v => v.Gender == VoiceGender.Male);
if (maleVoice != null)
{
    synth.Voice = maleVoice;
}

One issue that I had when trying to integrate this into my app is that in some cases I might want to have the main thread call the synthesizer multiple times to output a series of different messages. In my next post I will show how I used classes from System.Concurrent.Collections to queue up my synthesizer outputs.
