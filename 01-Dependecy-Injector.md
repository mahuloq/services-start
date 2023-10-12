Create a service

logging.service.ts
export class LoggingService {
logStatusChange(status: string) {
console.log('A server status changed, new status: ' + status);
}
}

use in component.ts

import { Component, EventEmitter, Output } from '@angular/core';
import { LoggingService } from '../logging.service';

@Component({
selector: 'app-new-account',
templateUrl: './new-account.component.html',
styleUrls: ['./new-account.component.css'],
providers: [LoggingService],
})
export class NewAccountComponent {
@Output() accountAdded = new EventEmitter<{ name: string; status: string }>();

constructor(private loggingService: LoggingService) {}

onCreateAccount(accountName: string, accountStatus: string) {
this.accountAdded.emit({
name: accountName,
status: accountStatus,
});
this.loggingService.logStatusChange(accountStatus);
}
}
