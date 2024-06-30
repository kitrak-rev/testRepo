WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
            wait.until((ExpectedCondition<Boolean>) wd ->
                ((JavascriptExecutor) wd).executeScript("return document.readyState").equals("complete"));

            // Now wait for the updated element to be present
            WebElement updatedElement = wait.until(ExpectedConditions.presenceOfElementLocated(By.id("updatedElement")));

            // Interact with the updated element
            System.out.println("Updated Element Tag Name: " + updatedElement.getTagName());
