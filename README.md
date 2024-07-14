    public static void waitForPageRefresh(WebDriver driver) {
        // Use JavaScript to wait for document.readyState to be complete after the refresh
        WebDriverWait wait = new WebDriverWait(driver, 10);
        wait.until((ExpectedCondition<Boolean>) wd ->
            ((JavascriptExecutor) wd).executeScript("return document.readyState").equals("complete"));
    }
