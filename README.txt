1. Service on web
	The frontend can be utilized via 'runserver' from django.
	Usage: python manage.py runserver 0:8000
	
	However, it is recommended to use other web server to put this frontend on the web. (e.g., apache) In practice, I
	used apache to do this and the virtual profile settings is in the '/etc/apache2/sites-available/000-default.conf'.

	In '000-default.conf', the frontend is set to run on VirtualHost with port 8000, where the default web is serviced on
	port 80. Since we have to run a Host on port 80 (embio or other default domain), the frontend have to use different
	port which is open from outside.

	In order to avoid messy port in URL, such as 'example.com:8000/frontend', I use 'ProxyPass' in the original Host
	(e.g., example.com). It helps to redirect to the frontend without loss of domain and port in URL.
	Usage: ProxyPass /frontend http://example.com:8000/frontend, and ProxyPassReserve

	Another advantages of using apache is convenience of handling virtual environments. It can be utilized via
	WSGIDaemonProcess, WSGIProcessGroup, WSGIScriptAlias statement in '000-default.conf'. As a result, we can use
	virtual envs without even activating it. (may be different in pyvenv)


2. STATIC_URL in settings.py
	The default STATIC_URL is 'static/'.
	However, I changed it to the absolute path to access in redirected page because static files cannot be accessed in
	redirected page (throughd 'ProxyPass').  It can be improved other method.
	Original: STATIC_URL = 'static/'
	Changed: STATIC_URL = 'domain.com:PORT_NUM/DIR_NAME/'

