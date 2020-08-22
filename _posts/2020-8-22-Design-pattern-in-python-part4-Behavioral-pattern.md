---
layout: post
title: "Design pattern in python- part4  Behavioral Pattern"
description: "Design pattern - Behavioral pattern"
tags: [python, design-pattern]
# link: http://mademistakes.com  
published: true
authors: 'Bhoj Bahadur Karki'
name: 'Bhoj Bahadur Karki'
share: true
---

## Behavioral Pattern
Behavioral desing pattern is concerned communication between objects. Lets look at some of the examples of Behavioral pattern in this article. 

### Chain of responsibility desgin pattern:
The chain of responsibility pattern is used to achieve loose coupling in software where a
specified request from the client is passed through a chain of objects included in it. It helps in
building a chain of objects. The request enters from one end and moves from one object to another.Let's look at an example below:

{% highlight python %}
    class ReportFormat(object):
        PDF = 0
        TEXT = 1


    class Report(object):
        def __init__(self, format_):
            self.title = 'Monthly report'
            self.text = ['Things are going', 'really, really well.']
            self.format_ = format_


    class Handler(object):
        def __init__(self):
            self.nextHandler = None

        def handle(self, request):
            self.nextHandler.handle(request)


    class PDFHandler(Handler):

        def handle(self, request):
            if request.format_ == ReportFormat.PDF:
                self.output_report(request.title, request.text)
            else:
                super(PDFHandler, self).handle(request)

        def output_report(self, title, text):
            print('<html>')
            print(' <head>')
            print(' <title>%s</title>' % title)
            print(' </head>')
            print(' <body>')
            for line in text:
                print(' <p>%s</p>' % line)
            print(' </body>')
            print('</html>')


    class TextHandler(Handler):

        def handle(self, request):
            if request.format_ == ReportFormat.TEXT:
                self.output_report(request.title, request.text)
            else:
                super(TextHandler, self).handle(request)

        def output_report(self, title, text):
            print(5 * '*' + title + 5 * '*')
            for line in text:
                print(line)


    class ErrorHandler(Handler):
        def handle(self, request):
            print("Invalid request")


    if __name__ == '__main__':
        report = Report(ReportFormat.TEXT)
        pdf_handler = PDFHandler()
        text_handler = TextHandler()

        pdf_handler.nextHandler = text_handler
        text_handler.nextHandler = ErrorHandler()
        pdf_handler.handle(report)
{% endhighlight %}
Output:

{% highlight shell %}
    *****Monthly report*****
    Things are going
    really, really well.
{% endhighlight %}

### Observer desgin pattern:
In this pattern, objects are represented as observers that wait for an event to trigger.Let's look at an example below:

{% highlight python %}
    import threading
    import time


    class Downloader(threading.Thread):

        def run(self):
            print("downloading...")
            for i in range(1, 5):
                self.i = i
                time.sleep(2)
                print("fun fun")
            return "Hellow world"


    class Worker(threading.Thread):
        def run(self):
            # print("worker")
            for i in range(1, 5):
                print("working running ... {}{} \n".format(i, t.i))
                time.sleep(1)
                t.join()

            print("done")


    t = Downloader()
    t.start()

    print("----------sleep---------")
    time.sleep(1)

    t1 = Worker()
    t1.start()

    t2 = Worker()
    t2.start()

    t3 = Worker()
    t3.start()

{% endhighlight %}
Output:

{% highlight shell %}
    ----------sleep---------
    downloading...
    working running ... 11 

    working running ... 11 

    working running ... 11 

    fun fun
    fun fun
    fun fun
    fun fun
    working running ... 24 
    working running ... 24 


    working running ... 24 

    working running ... 34 
    working running ... 34 


    working running ... 34 

    working running ... 44 

    working running ... 44 

    working running ... 44 

    done
    done
    done

{% endhighlight %}

### Conclusion
In conclusion, it is always a good idea to know about software design patter because it let's us find solution to common problem in efficient way. 

There are some criticism of design pattens as well. As an avid reader we should be aware of both the positive and negative aspect of design pattern. As quoted in [Wikepedia](https://en.wikipedia.org/wiki/Design_Patterns#Criticism) one criticism is layed out as " A primary criticism of Design Patterns is that its patterns are simply workarounds for missing features in C++, replacing elegant abstract features with lengthy concrete patterns, essentially becoming a "human compiler" or "generating by hand the expansions of some macro""
There are other criticism as well but as a developer it's more fruitful to know about these design pattern concepts as they are commonly used in day to day life.

You can also search for more patter implementation in the web as this article has only some major ones.