            ChromeOptions options = new ChromeOptions();
            HashMap<String, Object> prefs = new HashMap<>();
            prefs.put("profile.default_content_settings.popups", 0);
            prefs.put("download.default_directory", LocationUtil.getDownloadFolderPath());
            prefs.put("download.prompt_for_download", false);
            prefs.put("safebrowsing.enabled", true);
            options.setExperimentalOption("prefs", prefs);
            options.addArguments("start-maximized");
            options.addArguments("--safebrowsing-disable-download-protection");
            options.addArguments("safebrowsing-disable-extension-blacklist");

            // Set DesiredCapabilities
            DesiredCapabilities capabilities = new DesiredCapabilities();
            capabilities.setBrowserName("chrome");
            capabilities.setCapability(ChromeOptions.CAPABILITY, options);
