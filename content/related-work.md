## Related Work
{:#related-work}

This section lists relevant related work in the topics of LTQP and security.

### Link-Traversal-based Query Processing

Write me
{:.todo}

### Security Vulnerabilities

In this section, we describe related work on security vulnerabilities in different areas related to LTQP.

#### Web Crawlers

[Web crawling](cite:cites crawling) is a process that involves collecting information on the Web by following links between pages.
Web crawlers are typically used for Web indexing to aid search engines.
[Focused crawling](cite:cites focusedcrawling) is a special form of Web crawling that prioritizes certain Web pages,
such as Web pages about a certain topic, or domains for a certain country.
LTQP can therefore be considered as an area of focused crawling that where the priority lies in achieving query results.

Web crawlers are often used for discovering vulnerable Web sites,
for example through [_Google Dorking_](cite:cites googledorks),
which involves using Google Search to find Web sites that are misconfigured or use vulnerable software.
Furthermore, crawlers are often used to find private information on Web sites.
Such issues are however not the focus of this work.
Instead, we are interested in the security of the crawling process itself,
for which little research has been done to the best of our knowledge.

One related work in this area involves [abusing crawlers to initiate attacks on other Web sites](cite:cites crawlerattacks).
This may cause performance degradation on the attacked Web site,
or could even cause the crawling agent to be blocked by the server.
These attacks involve convincing the crawler to follow a link to a third-party Web site
that exploits a certain vulnerability, such as an SQL injection.
Additionally, this work describes a type of attack that allows vulnerable Web sites to be used
for improving the [PageRank](cite:cites pagerank) of an attacker-owned Web site via forged backlinks.

#### Web Browsers

Web browsers enable users to visualize and interact with Web pages.
This interaction is closely related to LTQP,
with the main difference that LTQP works autonomously,
while Web browsers are user-driven.
Considering this close resemblance between these two domains,
we give an overview of the main security vulnerabilities in Web browsers.

**Modern Web browser architecture:**

[Silic et al.](cite:cites securitymodernwebbrowserarchitecture)
analyzed the architectures of modern Web browsers,
determined the main vulnerabilities,
and discuss how these issues are coped with.

Architecture-wise, browsers can be categorized into monolithic and modular browser architectures.
The difference between the two is that the former does not provide isolation between concurrently executed Web programs, while the latter does.
The authors argue that a modular architecture is important for security, fault-tolerance and memory management.
They focused on the security aspects of the [Chrome browser architecture](cite:cites securitychromium),
which consist of separate modules for the rendering engine, browser kernel, and plugins.
Each of these modules is isolated in its own operating system process.

Silic et al. list the following main threats for Web browsers:

1. **System compromise**: Malicious arbitrary code execution with full privileges on behalf of the user. For example, exploits in the browser or third-party plugins caused by bugs. These types of attacks are mitigated through automatic updates once exploits become known.
2. **Data theft**: Ability to steal local network or system data. For example, a Web page includes a subresource to URLs using the file scheme (`file://`). which are usually blocked.
3. **Cross domain compromise**: Code from a Fully Qualified Domain Name (FQDN) executes code (or reads data) from another FQDN. For example, a malicious domain could extract authentication cookies from your bank's website you are logged into. This is usually blocked through the same-origin policy, but can be explicitly allowed through [Cross-Origin Resource Sharing (CORS)](https://fetch.spec.whatwg.org/#http-cors-protocol){:.mandatory}.
4. **Session hijacking**: Session tokens are compromised through theft or session token prediction. For example, [cross-domain request forgery (CSRF)](cite:cites csrf) is a type of attack that involves an attacker forcing a user logged in on another Web site to perform an action without their consent. Web browsers do not protect against these, but are typically handled by Web frameworks via the [Synchronizer Token Pattern](cite:cites synchronizertokenpattern).
5. **User interface compromise**: Manipulating the user interface to trick the user into performing an action without their knowledge. For example, placing an invisible button in front of another button. This category also includes CPU and memory hogging to block the user from taking any further actions. Web browser have limited protections for these types of attacks that involve placing limitations on user interface manipulations.

**Lessons from Google Chrome:**

[Reis et al.](cite:cites securitychromelessons) discuss on the three problems Google Chrome developers focus on to mitigate attacks:

1. **Reducing vulnerability severity**: In the real world, large projects such as Web browsers always contain bugs. Given this reality, Google Chrome consists of several sandbox layers reducing the damage should an exploit be discovered in one of the layers. The difficulty here lies in the fact that Web compatibility should be maintained, so that security restrictions do not break people's favorite Web sites.
2. **Reducing window of vulnerability**: If an exploit has been discovered, it should be patched as soon as possible. Google Chrome follows automated testing to ship security patches as soon as possible. All existing Chrome installations check for updates every five hours, and update in the background without disrupting the user experience.
3. **Reducing frequency of exposure**: In order to avoid people from visiting malicious Web sites for which the browser has not been patched yet, Google Chrome makes use of a continuously updating database of such Web sites. This will show a warning to the user before visiting such a site.

**SQL Injection**

[SQL injection attacks](cite:cites sqlinjection) are one of the most common vulnerabilities on Web sites
where (direct or indirect) user input is not properly handled, and may lead to the attacker performing unintended SQL statements on databases.
These types of attacks are typically mitigated through strong input validation, which are typically available in reusable libraries.

**Techniques for mitigating browser vulnerabilities:**

[Browser Hardening](cite:cites hardening) is based on the concept of reducing privileges of browsers to increase security.
For example, browsers can be configured to disabled JavaScript and Adobe Flash, or whitelisted to trusted Web sites.

[Fuzzing](cite:cites fuzzing) is a technique that involves generating random data as input to software.
Major Web browsers such as Google Chrom and Microsoft Edge perform extensive fuzzed testing by generating random Web pages and running them through the browser to detect crashes and other vulnerabilities.

#### RDF Query Processing 

Research involving the security vulnerabilities of RDF query processing
has been primarily focused on injection attacks within Web applications
that internally send SPARQL queries to a SPARQL endpoint.
So far, no research has been done on vulnerabilities specific to RDF federated querying or link traversal.
As such, we list the relevant work on single-source SPARQL querying hereafter.

The most significant type of security vulnerability in Web applications is _Injection through User Input_,
of which [SQL injection attacks](cite:cites sqlinjection) are a primary example.
[Orduna et al.](cite:cites sparqlinjectionattacks) investigate this type of attack in the context of SPARQL queries,
and show that parameterized queries can help avoid this type of attacks.
The authors implemented parameterized queries in the [Jena framework](cite:cites jena) as a mitigation example.

[SemGuard](cite:cites semguard) is a system that aims to detect injection attacks in both SPARQL and SQL queries for query engines that support both.
A motivation of this work is that the use of parameterized queries is not always possible,
as systems may already have been implemented without them,
and updating them would be too expensive.
This approach is based on the automatic analysis of the incoming query's parse tree.
It will check if the parse tree only has a leaf node for the expected user input, compared to the original template query's parse tree.
If it does not have a leaf node, this means that the user is attempting to execute queries that were not intended by the application developer.

[Asdhar et al.](cite:cites insecurysemwebframework) analyzed injection attacks to Web applications
via the [SPARQL query language](cite:cites spec:sparqllang) and the [SPARQL update language](cite:cites spec:sparqlupdate).
Furthermore, they provide _SemWebGoat_, a deliberately insecure RDF-based Web application for educational purposes around security.
All of the discussed attacks involve some form of injection,
leading to retrieval or modification of unwanted data,
or denial-of-service by for example injecting the `?s ?p ?o` pattern.