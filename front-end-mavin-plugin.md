### pox.xml

```xml
<!-- START packaging spring and client app in one jar -->
			<plugin>
				<groupId>com.github.eirslett</groupId>
				<artifactId>frontend-maven-plugin</artifactId>
				<version>1.12.0</version>
				<configuration>
					<workingDirectory>frontend</workingDirectory>
					<installDirectory>target</installDirectory>
				</configuration>
				<executions>
					<execution>
						<id>install node and npm</id>
						<goals>
							<goal>install-node-and-npm</goal>
						</goals>
						<configuration>
							<nodeVersion>v14.17.0</nodeVersion>
							<npmVersion>6.14.14</npmVersion>
						</configuration>
					</execution>
					<execution>
						<id>npm install</id>
						<goals>
							<goal>npm</goal>
						</goals>
						<configuration>
							<arguments>install</arguments>
						</configuration>
					</execution>
					<execution>
						<id>npm run build</id>
						<goals>
							<goal>npm</goal>
						</goals>
						<configuration>
							<arguments>run build</arguments>
						</configuration>
					</execution>
				</executions>
			</plugin>
			<plugin>
				<artifactId>maven-antrun-plugin</artifactId>
				<executions>
					<execution>
						<phase>generate-resources</phase>
						<configuration>
							<target>
								<copy todir="${project.build.directory}/classes/public">
									<fileset dir="${project.basedir}/frontend/dist/ngrx-course"/>
								</copy>
							</target>
						</configuration>
						<goals>
							<goal>run</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
			<!-- END packaging spring and client app in one jar -->
```

### config

```java
package com.example.superheroes.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.core.io.ClassPathResource;
import org.springframework.core.io.Resource;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.resource.PathResourceResolver;

import java.io.IOException;

@Configuration
public class MvcConfig implements WebMvcConfigurer {
	@Override
	public void addResourceHandlers(ResourceHandlerRegistry registry) {
		registry.addResourceHandler("/\*\*")
			.addResourceLocations("classpath:/public/")
			.resourceChain(true)
			.addResolver(new PathResourceResolver() {
				@Override
				protected Resource getResource(String resourcePath, Resource location) throws IOException {
					Resource requestedResource = location.createRelative(resourcePath);

											return requestedResource.exists() && requestedResource.isReadable() ? requestedResource
													: new ClassPathResource("/public/index.html");
									}
							});
		}

}
```

#### Packaging Locally

- use your preferred Java version
- you can use SDKMan for sdk and java versions management
- don't forget to install maven
- move angular project to the root directory of Spring Boot 2 named frontend or client-app or whatever
- go to root directory of Spring Boot 2 and run the below commands

```zsh
mvn clean
mvn package
```

- go to target directory and run the below commands

```zsh
java -jar target/name-of-your-apps-jar-file-0.0.1.jar
```

or just run your IntelliJ IDEA

- check localhost:8080
