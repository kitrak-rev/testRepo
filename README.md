```
import org.springframework.core.io.Resource;
import org.springframework.core.io.ResourceLoader;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

import java.io.IOException;
import java.nio.file.Files;

@Controller
public class StaticContentController {

    private final ResourceLoader resourceLoader;

    public StaticContentController(ResourceLoader resourceLoader) {
        this.resourceLoader = resourceLoader;
    }

    @GetMapping("/files/{filename}")
    public ResponseEntity<byte[]> serveFile(@PathVariable String filename) {
        try {
            // Load the file as a resource from the classpath or filesystem
            Resource resource = resourceLoader.getResource("classpath:/static/" + filename);

            // Read file content into a byte array
            byte[] fileContent = Files.readAllBytes(resource.getFile().toPath());

            // Determine the content type (MIME type) for the file
            String contentType = Files.probeContentType(resource.getFile().toPath());

            // Set response headers
            HttpHeaders headers = new HttpHeaders();
            headers.add(HttpHeaders.CONTENT_TYPE, contentType);

            // Return the file content as a ResponseEntity
            return new ResponseEntity<>(fileContent, headers, HttpStatus.OK);

        } catch (IOException e) {
            // If the file is not found or an error occurs, return 404
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }
    }
}
```
