---
layout: post
title: "Cookiereports done right"
description: ""
category: 
tags: csharp, razor, cookies, tips
excerpt_separator: "</p>"
---
{% include JB/setup %}

Err... sorta. There's no doubt that <a href="http://cookiereports.com">Cookiereports</a> is a terrible product, that I really can't see the value of; as opposed to writing the legal stuff yourself.
But for some reason client's insists on using them, so below you'll find a nice little Umbraco MacroPartial, that will at least let you integrate it into your sift a bit better than the god awful iframe solution they normally force you to use.
Naturally this solution isn't limited to Umbraco, it can be used in any MVC View or Partial View, or even in Webforms, if you care to do some minor coding yourself.

### The Razor part
What's happening below is actually quite trivial. It fetches the source for the page that normally would be embedded via an iframe, and runs a couple of Regex's against it, to extract the body, and transform all href's and src's to fully qualified Uri's, since we're no longer executing in the original domain context.
{% highlight csharp linenos %}
{% raw %}
@using System.Text
@using System.Text.RegularExpressions
@inherits Umbraco.Web.Macros.PartialViewMacroPage
@{
    var language = Model.MacroParameters["lang"].ToString();
    var client = new WebClient();
    var html = Encoding.UTF8.GetString(client.DownloadData("http://policy.cookiereports.com/aea84c19-" + language + ".html"));
    var r = new Regex(@"<body[^>]*>(?<content>.*)</body>", RegexOptions.IgnoreCase | RegexOptions.Singleline);
    var body = r.Match(html).Groups["content"].Value;
    r = new Regex(@"(?<attr>src|href)=""(?<val>[^""]*)""", RegexOptions.IgnoreCase | RegexOptions.Singleline);
    body = r.Replace(body, @"${attr}=""http://policy.cookiereports.com/${val}""");
}
@Html.Raw(body)
{% endraw %}
{% endhighlight %}

### The CSS part
This is an almost exact copy of the original CSS, with only a few changes. Everything is now properly namespaced under #policy, to prevent inteference, and url's are now pointing on fully qualified Uri's.
{% highlight css linenos %}
{% raw %}
<style type="text/css">
    #policy {
        font: 14px Arial,Helvetica,sans-serif;
        color: #553e3d;
    }

        #policy h1, #policy h2, #policy h3, #policy h4, #policy h5, #policy h6, #policy h1 a, #policy h2 a, #policy h3 a, #policy h4 a, #policy h5 a, #policy h6 a {
            color: #553e3d;
        }

        #policy h1 {
            font-size: 14px;
        }

        #policy h2 {
            font-size: 14px;
        }

        #policy h3 {
            font-size: 9.8px;
        }

        #policy h4 {
            font-size: 7.7px;
        }

        #policy h5 {
            font-size: 5.6px;
        }

        #policy h6 {
            font-size: 3.5px;
        }

        #policy h1, #policy h2, #policy p, #policy li {
            font-family: "Soho W02 Regular";
            font-size: 14px;
            line-height: 20px;
        }

        #policy .section {
            padding: 0 0 7px 0;
        }

        #policy ul.details li {
            padding: 10px 0 0 10px;
        }

    #policy-flash .screenshot {
        background: url('http://policy.cookiereports.com/i/policy/screenshot-flash') no-repeat;
        height: 300px;
    }

    #policy .pdficon {
        display: inline-block;
        background: url('http://policy.cookiereports.com/i/policy/pdf') no-repeat;
        height: 17px;
        width: 17px;
        margin-left: 4px;
    }

    #policy #policy-downloads .pdficon {
        margin: 0 4px 0 0;
    }

    #policy #policy-downloads ul {
        padding: 0;
        margin: 0;
    }

    #policy #policy-downloads li {
        list-style-type: none;
        padding: 0;
        margin: 3px 0;
    }

    #policy table.cookies {
        font-size: 0.8em;
    }

        #policy table.cookies th, table.cookies td {
            padding: 3px 9px;
        }

        #policy table.cookies th {
            vertical-align: top;
            text-align: left;
            border-bottom: 1px solid #ccc;
        }

    #policy .rtl table.cookies th {
        vertical-align: top;
        text-align: right;
    }

    #policy table.cookies td {
        vertical-align: top;
        border-bottom: 1px dotted #ccc;
    }

    #policy tr.info td {
        border-bottom: 1px solid #ccc;
    }

    #policy td.description p {
        margin-top: 0;
        padding-top: 0;
    }

    #policy td.icon .icon {
        float: left;
        margin-right: 6px;
    }

    #policy .rtl td.icon .icon {
        float: right;
        margin-left: 6px;
    }

    #policy tr.green td.icon div.icon {
        background-image: url('/i/policy/green/led');
        background-position: 0 0;
        width: 15px;
        height: 15px;
    }

    #policy tr.amber td.icon div.icon {
        background-image: url('/i/policy/green/led');
        background-position: -16px 0;
        width: 15px;
        height: 15px;
    }

    #policy tr.red td.icon div.icon {
        background-image: url('/i/policy/green/led');
        background-position: -32px 0;
        width: 15px;
        height: 15px;
    }


    #policy dl {
        margin: 10px 100px 30px 60px;
    }

    #policy dt {
        float: left;
        font-weight: bold;
    }

    #policy dd {
        margin-left: 190px;
        margin-bottom: 10px;
    }

    #policy .policy-icon-heading {
        color: inherit;
        text-decoration: none;
    }

    #policy .toggle-icon {
        margin-right: 6px;
    }
</style>
{% endraw %}
{% endhighlight %}

### The jQuery part
This part is written from scratch to mimic the functionality of the original JavaScript provided by Cookiereports themselves. The reason for this is that aside from being very clunky, and unnecessarily complex, I had to change it anyway to recity the img src.
{% highlight javascript linenos %}
{% raw %}
<script type="text/javascript">
    $(function () {
        $("#policy .sections .section").each(function () {
            var $this = $(this), $h2 = $this.find("h2"), $desc = $this.find("*[id$='_cookies']");
            if ($desc.length > 0) {
                $h2.click(function () {
                    $desc.toggle();
                    var $img = $h2.find("img");
                    $img.attr("src", $desc.is(":visible") ? "http://policy.cookiereports.com/i/boxminus.png" : "http://policy.cookiereports.com/i/boxplus.png");
                }).prepend("<img class=\"plusminus\" src=\"http://policy.cookiereports.com/i/boxplus.png\" />&nbsp;&nbsp;").css("cursor", "pointer");
                $desc.css("display", "none");
            }
        });
    });
</script>
{% endraw %}
{% endhighlight %}

I hope you find it as useful as I did. And while we can easily agree that they should've provided us proper solution, and not let us do symptom treatment, at least it'll get the job done.