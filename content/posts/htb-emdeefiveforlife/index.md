+++
title = "Emdee Five For Life"
date = 2019-06-17
[taxonomies]
tags = ["hackthebox"]
+++

“Can you encrypt fast enough?”

This was my first challenge on HTB. It was fun and a little bit challenging. I couldn't write this in Python as it was annoying for something this simple. I should do it in Python though so at least I understand how to write HTTP responses. In Ruby, I had less of trouble as the documentation is better and understandable. Overall, this is a nice challenge to get warmed up for other challenges to come.

What I could have improved on is the if else could be more precise when it finds the flag. I didn't realized the flag was a specific string until I submitted the flag. Next time, I will know what to look for.

```ruby
require 'net/http'
require 'uri'
require 'digest'
 
uri = URI.parse("http://docker.hackthebox.eu")
uri.scheme = "http"
uri.host = "docker.hackthebox.eu"
uri.port = 35404
uri.path = "/"
 
http = Net::HTTP::new(uri.host, uri.port)
 
i = 0
 
while i <= 10 do
 
puts "Loop #: " + i.to_s
 
	get_request = Net::HTTP::Get.new(uri.request_uri)
	get_response = http.request(get_request)
 
	get_string = /[A-Za-z0-9]{20}/.match(get_response.body)
 
	puts "Captured string: " + get_string.to_s
 
	cookies = get_response.response['set-cookie']
 
	encrypt_string_md5 = Digest::MD5.hexdigest(get_string.to_s)
 
	puts "Encrypted MD5: " + encrypt_string_md5
 
	post_request = Net::HTTP::Post.new(uri.request_uri)
 
	post_request.set_form_data({"hash" => encrypt_string_md5})
 
	post_request['Cookie'] = cookies
 
	post_response = http.request(post_request)
 
	get_post_response = /Too\sslow!/.match(post_response.body)
 
	if get_post_response.nil? == false
		puts get_post_response.to_s + "\n\n"
	else
		puts post_response.body
	end
 
i += 1
 
end
```

Useful link: [https://yukimotopress.github.io/http](https://yukimotopress.github.io/http)
