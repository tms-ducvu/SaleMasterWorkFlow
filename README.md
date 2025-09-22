
# n8n Source Control

- Cài đặt customize node: n8n-nodes-mcp
    - Github: https://github.com/nerding-io/n8n-nodes-mcp
    - Set enviroment (cmd - windows): ``setx N8N_COMMUNITY_PACKAGES_ALLOW_TOOL_USAGE "true"``
- MCP server: https://github.com/modelcontextprotocol/servers
---
---


# Trick N8N
### Windows
- B1: Windows + R => nhập **%appdata%** => Enter.
- B2: Truy cập đến file `AppData\Roaming\npm\node_modules\n8n\dist\license.js`  
    => Sửa các functions sau:

```
isLicensed(feature) {
    return true;
}

isAPIDisabled() {
    return false;
}

getTriggerLimit()
getVariablesLimit()
getAiCredits()
getTeamProjectLimit()
getUsersLimit() {
    return 999999;
}

getPlanName() {
    return "Enterprise"
}
```

- B4: Truy cập đến file `AppData\Roaming\npm\node_modules\n8n\node_modules\@n8n\constants\dist\index.d.ts`  
    => Sửa biến sau:  
    `export declare const UNLIMITED_LICENSE_QUOTA = 999999;`
- B5: Truy cập đến file `AppData\Roaming\npm\node_modules\n8n\dist\services\project.service.ee.js`  
    => Comment 3 dòng sau trong function:
```
async createTeamProjectWithEntityManager(adminUser, data, trx) {
    const limit = this.licenseState.getMaxTeamProjects();
    if (limit !== constants_1.UNLIMITED_LICENSE_QUOTA) {
        const teamProjectCount = await trx.count(db_1.Project, { where: { type: 'team' } });
        // if (teamProjectCount >= limit) {
        //     throw new TeamProjectOverQuotaError(limit);
        // }
    }
    const project = await trx.save(db_1.Project, this.projectRepository.create({ ...data, type: 'team' }));
    await this.addUser(project.id, { userId: adminUser.id, role: 'project:admin' }, trx);
    return project;
}
```

### Linux
- Tìm đến folder cài đặt N8N rồi thực hiện tương tự