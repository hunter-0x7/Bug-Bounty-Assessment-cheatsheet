Top Level Domain
	Target.com
		List of IPs
Recon and Mapping
	WordLists
		Custom Word lists
			Cewl
				$ cewl -m 4 -w dict.txt https://site.url
		JHaddix's all.txt
			https://gist.github.com/jhaddix/f64c97d0863a78454e44c2f7119c2a6a
		SecLists' raft-large-words.txt
			https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/raft-large-words.txt
	List Subdomains
		Check for Wildcards first - to eliminate false positives
		knock
			https://github.com/guelfoweb/knock
		Sublist3r
			https://github.com/aboul3la/Sublist3r
		LazyRecon
			https://github.com/capt-meelo/LazyRecon
		subfinder
			https://github.com/ice3man543/subfinder
		amass
			https://github.com/OWASP/Amass
		ReconDog
			https://github.com/s0md3v/ReconDog
		subbrute
			https://github.com/TheRook/subbrute
		Cloudflare Enumeration Tool
			https://github.com/mandatoryprogrammer/cloudflare_enum
		dnsmap
			https://github.com/makefu/dnsmap
		altdns
			https://github.com/infosec-au/altdns
		dnsrecon
			https://github.com/darkoperator/dnsrecon
		dnssearch
			https://github.com/evilsocket/dnssearch
		Fierce
			https://github.com/mschwager/fierce
		second-order
			https://github.com/mhmdiaa/second-order
		assetfinder
			https://github.com/tomnomnom/assetfinder
		DNSDumpster
			https://dnsdumpster.com/
		Dig - Zone transfer
			dig [target URL] -r axfr
		Massdns
		Nmap
		GoogleDorks
		Shodan.io
			Search by Org:
	Other domains
		Reverse Whois
			viewdns info
				https://viewdns.info/reversewhois/
			reverse.report
				https://reverse.report/
		Nerdydata With Analytics UA
			https://nerdydata.com/reports/new
		Reverse IP Domain
			yougetsignal.com
				https://www.yougetsignal.com/tools/web-sites-on-web-server/
			Nmap reverse DNS
		(Sub)Domain running on same nameserver
			dns.coffee/nameservers
		Domains associated with target
			DomLink
				https://github.com/vysecurity/DomLink
		Expired Domains
			Domains Hunter
				https://github.com/threatexpress/domainhunter
	Discover new endpoints
		Wayback Machine
			https://archive.org/web/
		JSParcer
			https://github.com/nahamsec/JSParser
		cc.py
			https://github.com/si9int/cc.py
	Site Inspection
		Visual recon
			Aquatone
				https://github.com/michenriksen/aquatone
			EyeWitness
		Sitemap
		Robots.txt
		Dig - Any
	3rd Party
		Applications
		CDNs
		Payment Systems
		Webapp Store Fronts
		WAFs
		Scripts
			CSS
			.js
		Code Repo
			GitHub
				Github Dorks
					https://github.com/techgaun/github-dorks
				Email Finder - Zen
					https://github.com/s0md3v/zen
			gitlab
			bitbucket
			Jenkins
	Cloud Enumeration/Brute Force
		bucket_finder
			https://digi.ninja/projects/bucket_finder.php
		lazys3
			https://github.com/nahamsec/lazys3
		teh_s3_bucketeers
			https://github.com/tomdev/teh_s3_bucketeers
		Sandcastle
			https://github.com/0xSearches/sandcastle
		CloudScraper
			https://github.com/jordanpotti/CloudScraper
		Public buckets
			https://buckets.grayhatwarfare.com/
		Manual search
			[Bucket Name].s3.amazonaws.com
	Certificate Search
		crt.sh
			https://crt.sh/
		Facebook Certificate Transparency Monitoring
			https://developers.facebook.com/tools/ct/
		Certspotter API
			https://certspotter.com/api/v0/certs?domain=
	Technology Profile
		builtwith.com
			https://builtwith.com/
		wappalyzer.com
			https://www.wappalyzer.com/
		w3techs.com
			https://w3techs.com/sites
		toolbar.netcraft.com
			https://sitereport.netcraft.com/
		whatcms.org
			https://whatcms.org/
	Publicly Disclosed Vulnerabilities or Leaks
		Open Bug Bounty
			https://www.openbugbounty.org/
		hackerOne public disclosures
			https://hackerone.com/hacktivity
		Google
		Pastebin
