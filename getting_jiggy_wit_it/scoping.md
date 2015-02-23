# Scoping

We touched briefly on singleton and no scope objects in this section, so let's delve a little into what scoping is. WireBox's default behavior is to create a new instance of an object each time you request it via creation or injection (Transient/Prototype objects), this is the NO SCOPE scope. Scopes allow you to customize the object's life span and duration. The singleton scope allows for the creation of only one instance of an object that will live for the entire life span of the injector. WireBox ships with several different life span scopes but you can also create your own custom scopes (please see the custom scopes section). You can also tell WireBox in what scope to place the instance into by annotations or via the configuration binder. We have an entire section dedicated to discovering all the WireBox annotations, but let's get a sneak peek at them and also how to do it via our mapping DSL.

### Scope Annotations

* You can tag a cfcomponent tag or component declaration with a scope={named scope} annotation that tells WireBox what scope to use
* You can have nothing on the cfcomponent tag or component declaration which denotes the NO SCOPE
* You can tag a cfcomponent tag or component declaration with a singleton annotation


### Scope Configuration Binder

<div style="border: 1px dashed black">
<p style="margin:12px">component extends=<span style="color:blue">"wirebox.system.ioc.config.Binder"</span>{<br><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:red">function</span> configure(){<br><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:green">// map with shorthand or full scope notation</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;mapPath(<span style="color:blue">"model.CoffeeShop"</span>).asSingleton();<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;mapPath(<span style="color:blue">"model.CoffeeShop"</span>).into(<span style="color:red">this</span>.SCOPES.SINGLETON);<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:green">// map some long espresso into request scope</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;map(<span style="color:blue">"longEspress"</span>)<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.to(<span style="color:blue">"model.Espresso"</span>)<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.to(<span style="color:red">"this"</span>.SCOPE.REQUEST);<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:green">// cache some tea</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;map(<span style="color:blue">"GreenTea"</span>)<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.to(<span style="color:blue">"model.Tea"</span>)<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.inCacheBox(timeout=20,provider=(<span style="color:blue">"ehCache"</span>);<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:green">// cache some google news that refresh themselves every 40 min or after 20 min of inactivity</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;map(<span style="color:blue">"latestNews"</span>)<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.inCacheBox(timeout=20,provider=(<span style="color:blue">"ehCache"</span>);<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.toRSS(<span style="color:blue">"http://news.google.com/news?output=rss"</span>);</p>
</div>
<br>
<div style="border: 1px solid black">
<img src="../images/icon_info.png" width="8%" style="float:left;margin-top:3px"><p style="margin:12px"><b> Note: In the configuration binder section you will see where the this.SCOPES enumeration class comes from.</b></p>
<div style="clear:both"></div>
</div>
<br>
### Internal Scopes

Here are the internal scopes that ship with WireBox:
<table>
    <tr>
        <th>Scope</th>
        <th>Description</th>
    <tr>
    <tr>
        <td>NOSCOPE</td>
        <td>A prototype object that gets created every time it is requested.</td>
    </tr>
    <tr>
        <td>PROTOTYPE </td>
        <td>A prototype object that gets created every time it is requested.</td>
    </tr>
    <tr>
        <td>SINGLETON </td>
        <td>Only one instance of the object exists</td>
    </tr>
    <tr>
        <td>SESSION</td>
        <td>The object will exist in the session scope</td>
    </tr>
    <tr>
        <td>APPLICATION </td>
        <td>The object will exist in the application scope</td>
    </tr>
    <tr>
        <td>REQUEST </td>
        <td>The object will exist in the request scope</td>
    </tr>
    <tr>
        <td>SERVER </td>
        <td>The object will exist in the server scope</td>
    </tr>
    <tr>
        <td>CACHEBOX</td>
        <td>A object will be time persisted in any [CacheBox](http://wiki.coldbox.org/wiki/CacheBox.cfm) cache provider</td>
    </tr>
</table>

This is cool! We can now have full control of how objects are persisted via the WireBox injector, we are not constricted to one type of persistence anymore.
<br>
<div style="border: 1px solid black">
<img src="../images/icon_info.png" width="10%" style="float:left;margin-top:5px"><p style="margin:12px"><b> Important : If you use a persistence scope that expires after time like session, request, cachebox, etc, you will experience a side effect called scope widening injection. WireBox offers a solution to this side effect via WireBox Providers, which we will cover in detail. </b></p>
<div style="clear:both"></div>
</div>
<br>