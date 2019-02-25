### CDN

CDN stands for “Content Delivery Network” and the technology provides a 
way of serving assets such as static HTML, CSS, Javascript, and images 
over the web much faster than serving them from a single origin server. 
It works by distributing the content across many “edge” servers around 
the world so that users end up downloading assets from the “edge” 
servers instead of the origin server. For instance in the image below, a
user in Spain requests a web page from a site with origin servers in 
NYC, but the static assets for the page are loaded from a CDN “edge” 
server in England, preventing many slow cross-Atlantic HTTP requests.



![CDN](..\img\CDN.png)



In general a web app should always use a CDN to serve CSS, Javascript, 
images, videos and any other assets. Some apps might also be able to 
leverage a CDN to serve static HTML pages.