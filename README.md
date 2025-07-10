
import { useState } from 'react';
import { Button } from '@/components/ui/button';
import { Textarea } from '@/components/ui/textarea';
import { Card } from '@/components/ui/card';
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from '@/components/ui/select';
import { Badge } from '@/components/ui/badge';
import { Heart, Smile, Star, Sparkles, Send, Share2, Download, Palette } from 'lucide-react';
import { toast } from '@/hooks/use-toast';

const Index = () => {
  const [letter, setLetter] = useState('');
  const [selectedFont, setSelectedFont] = useState('handwriting');
  const [selectedPaper, setSelectedPaper] = useState('cream');
  const [selectedInk, setSelectedInk] = useState('blue');
  const [selectedStickers, setSelectedStickers] = useState<string[]>([]);

  const fonts = [
    { id: 'handwriting', name: 'Handwriting', class: 'font-handwriting' },
    { id: 'script', name: 'Script', class: 'font-script' },
    { id: 'arabic', name: 'Arabic', class: 'font-arabic' },
    { id: 'default', name: 'Simple', class: 'font-sans' },
  ];

  const papers = [
    { id: 'cream', name: 'Cream', class: 'bg-[hsl(var(--paper-cream))]' },
    { id: 'vintage', name: 'Vintage', class: 'bg-[hsl(var(--paper-vintage))]' },
    { id: 'rose', name: 'Rose', class: 'bg-[hsl(var(--paper-rose))]' },
    { id: 'white', name: 'Pure White', class: 'bg-white' },
  ];

  const inks = [
    { id: 'blue', name: 'Ink Blue', class: 'text-[hsl(var(--ink-blue))]' },
    { id: 'black', name: 'Classic Black', class: 'text-[hsl(var(--ink-black))]' },
    { id: 'purple', name: 'Royal Purple', class: 'text-[hsl(var(--ink-purple))]' },
  ];

  const stickers = [
    { id: 'heart', icon: Heart, name: 'Heart' },
    { id: 'smile', icon: Smile, name: 'Smile' },
    { id: 'star', icon: Star, name: 'Star' },
    { id: 'sparkles', icon: Sparkles, name: 'Sparkles' },
  ];

  const currentFont = fonts.find(f => f.id === selectedFont);
  const currentPaper = papers.find(p => p.id === selectedPaper);
  const currentInk = inks.find(i => i.id === selectedInk);

  const toggleSticker = (stickerId: string) => {
    setSelectedStickers(prev => 
      prev.includes(stickerId) 
        ? prev.filter(id => id !== stickerId)
        : [...prev, stickerId]
    );
  };

  const shareLetter = () => {
    const letterData = {
      text: letter,
      font: selectedFont,
      paper: selectedPaper,
      ink: selectedInk,
      stickers: selectedStickers
    };
    
    // In a real app, this would generate a shareable link
    const shareUrl = `https://inkfeelings.com/letter/${btoa(JSON.stringify(letterData))}`;
    navigator.clipboard.writeText(shareUrl);
    
    toast({
      title: "Letter ready to share! 💌",
      description: "Link copied to clipboard",
    });
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-blue-50/30 via-purple-50/20 to-pink-50/30 p-6">
      <div className="max-w-7xl mx-auto">
        {/* Header */}
        <div className="text-center mb-12">
          <h1 className="text-5xl md:text-7xl font-script text-primary mb-4 drop-shadow-sm">
            Ink & Feelings
          </h1>
          <p className="text-xl text-muted-foreground max-w-3xl mx-auto leading-relaxed">
            Write heartfelt letters in any language, decorate with love, and share the magic of handwritten connection
          </p>
        </div>

        <div className="grid lg:grid-cols-3 gap-8">
          {/* Customization Panel */}
          <div className="space-y-6">
            <Card className="p-8 gentle-shadow border-border/50 bg-card/80 backdrop-blur-sm">
              <h3 className="text-xl font-semibold mb-6 flex items-center gap-3 text-foreground">
                <Palette className="w-6 h-6 text-primary" />
                Customize Your Letter
              </h3>
              
              {/* Font Selection */}
              <div className="space-y-4 mb-6">
                <label className="text-sm font-medium text-foreground">Font Style</label>
                <Select value={selectedFont} onValueChange={setSelectedFont}>
                  <SelectTrigger className="border-border/60 bg-background/50">
                    <SelectValue />
                  </SelectTrigger>
                  <SelectContent>
                    {fonts.map(font => (
                      <SelectItem key={font.id} value={font.id}>
                        <span className={font.class}>{font.name}</span>
                      </SelectItem>
                    ))}
                  </SelectContent>
                </Select>
              </div>

              {/* Paper Selection */}
              <div className="space-y-4 mb-6">
                <label className="text-sm font-medium text-foreground">Paper Style</label>
                <div className="grid grid-cols-2 gap-3">
                  {papers.map(paper => (
                    <Button
                      key={paper.id}
                      variant={selectedPaper === paper.id ? "default" : "outline"}
                      size="sm"
                      onClick={() => setSelectedPaper(paper.id)}
                      className={`${paper.class} text-foreground border-border/40 hover:border-primary/40 transition-all duration-200`}
                    >
                      {paper.name}
                    </Button>
                  ))}
                </div>
              </div>

              {/* Ink Color */}
              <div className="space-y-4 mb-6">
                <label className="text-sm font-medium text-foreground">Ink Color</label>
                <div className="grid grid-cols-1 gap-2">
                  {inks.map(ink => (
                    <Button
                      key={ink.id}
                      variant={selectedInk === ink.id ? "default" : "outline"}
                      size="sm"
                      onClick={() => setSelectedInk(ink.id)}
                      className={`${ink.class} justify-start border-border/40 hover:border-primary/40 transition-all duration-200`}
                    >
                      {ink.name}
                    </Button>
                  ))}
                </div>
              </div>

              {/* Stickers */}
              <div className="space-y-4">
                <label className="text-sm font-medium text-foreground">Decorative Stickers</label>
                <div className="grid grid-cols-2 gap-3">
                  {stickers.map(sticker => {
                    const IconComponent = sticker.icon;
                    const isSelected = selectedStickers.includes(sticker.id);
                    return (
                      <Button
                        key={sticker.id}
                        variant={isSelected ? "default" : "outline"}
                        size="sm"
                        onClick={() => toggleSticker(sticker.id)}
                        className="flex items-center gap-2 border-border/40 hover:border-primary/40 transition-all duration-200"
                      >
                        <IconComponent className="w-4 h-4" />
                        {sticker.name}
                      </Button>
                    );
                  })}
                </div>
              </div>
            </Card>
          </div>

          {/* Letter Writing Area */}
          <div className="lg:col-span-2 space-y-8">
            <Card className={`p-10 ${currentPaper?.class} paper-texture min-h-[600px] relative overflow-hidden gentle-shadow border-border/30`}>
              {/* Decorative Stickers */}
              <div className="absolute inset-0 pointer-events-none overflow-hidden">
                {selectedStickers.map((stickerId, index) => {
                  const sticker = stickers.find(s => s.id === stickerId);
                  if (!sticker) return null;
                  const IconComponent = sticker.icon;
                  
                  const positions = [
                    'top-6 right-6',
                    'top-6 left-6', 
                    'bottom-6 right-6',
                    'bottom-6 left-6'
                  ];
                  
                  return (
                    <IconComponent
                      key={`${stickerId}-${index}`}
                      className={`w-10 h-10 text-primary/25 absolute ${positions[index % positions.length]} animate-pulse`}
                      style={{ animationDelay: `${index * 0.5}s` }}
                    />
                  );
                })}
              </div>

              <div className="relative z-10">
                <Textarea
                  value={letter}
                  onChange={(e) => setLetter(e.target.value)}
                  placeholder="Write your heartfelt message here... ✨
                  
سلام، كيف حالك؟ (Arabic is supported too!)

Express your feelings, share your thoughts, or simply say hello in any language you love."
                  className={`w-full min-h-[500px] border-none bg-transparent resize-none text-lg leading-loose ${currentFont?.class} ${currentInk?.class} placeholder:text-muted-foreground/50 focus:ring-0 focus:outline-none selection:bg-primary/20`}
                  dir="auto"
                />
              </div>
            </Card>

            {/* Action Buttons */}
            <div className="flex flex-wrap gap-4 justify-center">
              <Button 
                onClick={shareLetter}
                disabled={!letter.trim()}
                className="flex items-center gap-2 bg-primary hover:bg-primary/90 px-6 py-3 text-base gentle-shadow"
              >
                <Share2 className="w-5 h-5" />
                Share Letter
              </Button>
              
              <Button 
                variant="outline" 
                disabled={!letter.trim()}
                className="flex items-center gap-2 px-6 py-3 text-base border-border/60 hover:bg-accent/50"
              >
                <Download className="w-5 h-5" />
                Download
              </Button>
              
              <Button 
                variant="outline" 
                disabled={!letter.trim()}
                className="flex items-center gap-2 px-6 py-3 text-base border-border/60 hover:bg-accent/50"
              >
                <Send className="w-5 h-5" />
                Send via Email
              </Button>
            </div>

            {/* Sample Letters */}
            <div className="text-center mt-12">
              <p className="text-base text-muted-foreground mb-6">
                Need inspiration? Try these sample messages:
              </p>
              <div className="flex flex-wrap gap-3 justify-center">
                <Badge 
                  variant="secondary" 
                  className="cursor-pointer hover:bg-secondary/80 px-4 py-2 text-sm border-border/40 transition-all duration-200"
                  onClick={() => setLetter("Dear friend,\n\nI hope this letter finds you well. I wanted to take a moment to tell you how much you mean to me...")}
                >
                  Friendship Letter
                </Badge>
                <Badge 
                  variant="secondary" 
                  className="cursor-pointer hover:bg-secondary/80 px-4 py-2 text-sm border-border/40 transition-all duration-200"
                  onClick={() => setLetter("عزيزي/عزيزتي،\n\nأكتب لك هذه الرسالة من القلب، لأخبرك كم أشتاق إليك...")}
                >
                  Arabic Sample
                </Badge>
                <Badge 
                  variant="secondary" 
                  className="cursor-pointer hover:bg-secondary/80 px-4 py-2 text-sm border-border/40 transition-all duration-200"
                  onClick={() => setLetter("My dearest,\n\nEvery day I think of you, and every day my heart grows fonder. This digital letter carries all my love...")}
                >
                  Love Letter
                </Badge>
              </div>
            </div>
          </div>
        </div>

        {/* Footer */}
        <div className="text-center mt-16 text-muted-foreground space-y-2">
          <p className="text-base">Made with ❤️ for bringing hearts closer together</p>
          <p className="text-sm">Free • Non-profit • Open Source</p>
        </div>
      </div>
    </div>
  );
};

export default Index;
