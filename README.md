ChromeOptions options = new ChromeOptions();
HashMap<String, Object> prefs = new HashMap<>();
prefs.put("profile.default_content_settings.popups", 0);
prefs.put("download.default_directory", LocationUtil.getDownloadFolderPath());
prefs.put("download.prompt_for_download", false);
prefs.put("safebrowsing.enabled", true);
options.setExperimentalOption("prefs", prefs);
ChromeDriver chromeDriver= new ChromeDriver(options);
ChromeOptions options = new ChromeOptions();
options.setExperimentalOption("prefs", prefs);
options.addArguments("start-maximized");
options.addArguments("--safebrowsing-disable-download-protection");
options.addArguments("safebrowsing-disable-extension-blacklist");
WebDriver driver =  new ChromeDriver(options); 
driver.get("http://www.landxmlproject.org/file-cabinet");
new WebDriverWait(driver, 20).until(ExpectedConditions.elementToBeClickable(By.xpath("//span[text()='MntnRoad.xml']//following::span[1]//a[text()='Download']"))).click();
