# nestjs-agenda
> The Agenda module you've been looking for

I've always wanted a module similar to @nestjs/schedule & @nestjs/bull but for [agenda](https://github.com/agenda/agenda), the mongoose task scheduling library. Stay tuned for a BullMQ nestjs module as well, because they sure as hell won't do it.


## Installation
```sh
yarn add @irfanreza-memberid/nestjs-agenda agenda
# npm install ...
```

## Usage
`app.processor.ts`
```ts
import { Processor, Define, Every, Schedule } from "@irfanreza-memberid/nestjs-agenda";
import { Job } from "agenda";

interface ISayYourName {
  name: string;
}

@Processor()
export class AppProcessor {
  @Define('Say "Hello world!"')
  @Every('30 seconds')
  sayHelloWorld() {
    console.log("Hello world!");
  }

  @Define('Say "Goodbye world!"')
  @Schedule('in 1 minute')
  sayGoodbyeWorld() {
    console.log("Goodbye world!")
  }

  @Define('Say your name')
  sayYourName(job: Job<ISayYourName>) {
    console.log(`Your name is ${job.attrs.data.name}`);
  }
}
```
`app.service.ts`
```ts
...
import { AgendaService } from "@irfanreza-memberid/nestjs-agenda";
@Injectable()
export class AppService {
  constructor(
    // AgendaService is an instance of Agenda
    private readonly agendaService: AgendaService,
  )
  
  sayYourName(name: string) {
    agendaService.now('Say your name', { name })
  }
}
```
`app.module.ts`
```ts
...
import { AgendaModule } from "@irfanreza-memberid/nestjs-agenda";
import { AppProcessor } from "./app.processor";

@Module({
  imports: [
    AgendaModule.forRoot({
      db: { 
        address: ... // Enter MongoDB URI Connection
      }
    })
  ],
  providers: [AppProcessor]
})
export class AppModule {}
```