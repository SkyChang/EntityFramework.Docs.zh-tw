---
title: EF Core 3.0 的中斷性變更 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: f7f04efa8fb8ebc1eb06f256b8ccbd3110af47ab
ms.sourcegitcommit: 705e898b4684e639a57c787fb45c932a27650c2d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2019
ms.locfileid: "71934875"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="0b84f-102">EF Core 3.0 中包含的重大變更</span><span class="sxs-lookup"><span data-stu-id="0b84f-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="0b84f-103">下列 API 和行為變更可能會在將現有的應用程式升級至3.0.0 時中斷。</span><span class="sxs-lookup"><span data-stu-id="0b84f-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="0b84f-104">這些變更預期只會影響[提供者變更](xref:core/providers/provider-log)底下記載的資料庫提供者。</span><span class="sxs-lookup"><span data-stu-id="0b84f-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="0b84f-105">總結</span><span class="sxs-lookup"><span data-stu-id="0b84f-105">Summary</span></span>

| <span data-ttu-id="0b84f-106">**重大變更**</span><span class="sxs-lookup"><span data-stu-id="0b84f-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="0b84f-107">**產生**</span><span class="sxs-lookup"><span data-stu-id="0b84f-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="0b84f-108">不會再於用戶端評估 LINQ 查詢</span><span class="sxs-lookup"><span data-stu-id="0b84f-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="0b84f-109">高</span><span class="sxs-lookup"><span data-stu-id="0b84f-109">High</span></span>       |
| [<span data-ttu-id="0b84f-110">EF Core 3.0 以 .NET Standard 2.1 為目標，而非以 .NET Standard 2.0 為目標</span><span class="sxs-lookup"><span data-stu-id="0b84f-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="0b84f-111">高</span><span class="sxs-lookup"><span data-stu-id="0b84f-111">High</span></span>      |
| [<span data-ttu-id="0b84f-112">EF Core 命令列工具 dotnet ef 不再是 .NET Core SDK 的一部分</span><span class="sxs-lookup"><span data-stu-id="0b84f-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="0b84f-113">高</span><span class="sxs-lookup"><span data-stu-id="0b84f-113">High</span></span>      |
| [<span data-ttu-id="0b84f-114">DetectChanges 接受存放區產生的索引鍵值</span><span class="sxs-lookup"><span data-stu-id="0b84f-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="0b84f-115">高</span><span class="sxs-lookup"><span data-stu-id="0b84f-115">High</span></span>      |
| [<span data-ttu-id="0b84f-116">FromSql、ExecuteSql 和 ExecuteSqlAsync 已重新命名</span><span class="sxs-lookup"><span data-stu-id="0b84f-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="0b84f-117">高</span><span class="sxs-lookup"><span data-stu-id="0b84f-117">High</span></span>      |
| [<span data-ttu-id="0b84f-118">查詢類型已與實體類型合併</span><span class="sxs-lookup"><span data-stu-id="0b84f-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="0b84f-119">高</span><span class="sxs-lookup"><span data-stu-id="0b84f-119">High</span></span>      |
| [<span data-ttu-id="0b84f-120">Entity Framework Core 不再屬於 ASP.NET Core 共用架構</span><span class="sxs-lookup"><span data-stu-id="0b84f-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="0b84f-121">Medium</span><span class="sxs-lookup"><span data-stu-id="0b84f-121">Medium</span></span>      |
| [<span data-ttu-id="0b84f-122">根據預設，串聯刪除現在會立即發生</span><span class="sxs-lookup"><span data-stu-id="0b84f-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="0b84f-123">Medium</span><span class="sxs-lookup"><span data-stu-id="0b84f-123">Medium</span></span>      |
| [<span data-ttu-id="0b84f-124">相關實體的積極式載入現在會出現在單一查詢中</span><span class="sxs-lookup"><span data-stu-id="0b84f-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="0b84f-125">Medium</span><span class="sxs-lookup"><span data-stu-id="0b84f-125">Medium</span></span>      |
| [<span data-ttu-id="0b84f-126">DeleteBehavior.Restrict 具有更簡潔的語意</span><span class="sxs-lookup"><span data-stu-id="0b84f-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="0b84f-127">Medium</span><span class="sxs-lookup"><span data-stu-id="0b84f-127">Medium</span></span>      |
| [<span data-ttu-id="0b84f-128">自有類型關聯性的設定 API 已變更</span><span class="sxs-lookup"><span data-stu-id="0b84f-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="0b84f-129">Medium</span><span class="sxs-lookup"><span data-stu-id="0b84f-129">Medium</span></span>      |
| [<span data-ttu-id="0b84f-130">各個屬性會使用獨立的記憶體內部整數索引鍵產生</span><span class="sxs-lookup"><span data-stu-id="0b84f-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="0b84f-131">Medium</span><span class="sxs-lookup"><span data-stu-id="0b84f-131">Medium</span></span>      |
| [<span data-ttu-id="0b84f-132">無追蹤查詢已不再執行身分識別解析</span><span class="sxs-lookup"><span data-stu-id="0b84f-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="0b84f-133">Medium</span><span class="sxs-lookup"><span data-stu-id="0b84f-133">Medium</span></span>      |
| [<span data-ttu-id="0b84f-134">中繼資料 API 變更</span><span class="sxs-lookup"><span data-stu-id="0b84f-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="0b84f-135">Medium</span><span class="sxs-lookup"><span data-stu-id="0b84f-135">Medium</span></span>      |
| [<span data-ttu-id="0b84f-136">提供者獨有的中繼資料 API 變更</span><span class="sxs-lookup"><span data-stu-id="0b84f-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="0b84f-137">Medium</span><span class="sxs-lookup"><span data-stu-id="0b84f-137">Medium</span></span>      |
| [<span data-ttu-id="0b84f-138">已移除 UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="0b84f-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="0b84f-139">Medium</span><span class="sxs-lookup"><span data-stu-id="0b84f-139">Medium</span></span>      |
| [<span data-ttu-id="0b84f-140">無法撰寫與預存程式搭配使用時的 FromSql 方法</span><span class="sxs-lookup"><span data-stu-id="0b84f-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="0b84f-141">Medium</span><span class="sxs-lookup"><span data-stu-id="0b84f-141">Medium</span></span>      |
| [<span data-ttu-id="0b84f-142">FromSql 方法只能在查詢根目錄上指定</span><span class="sxs-lookup"><span data-stu-id="0b84f-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="0b84f-143">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-143">Low</span></span>      |
| [<span data-ttu-id="0b84f-144">~~查詢執行會在偵錯層級記錄~~已還原</span><span class="sxs-lookup"><span data-stu-id="0b84f-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="0b84f-145">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-145">Low</span></span>      |
| [<span data-ttu-id="0b84f-146">實體執行個體上不會再設定暫存索引鍵值</span><span class="sxs-lookup"><span data-stu-id="0b84f-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="0b84f-147">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-147">Low</span></span>      |
| [<span data-ttu-id="0b84f-148">現在可以選用與主體共用資料表的相依實體</span><span class="sxs-lookup"><span data-stu-id="0b84f-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="0b84f-149">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-149">Low</span></span>      |
| [<span data-ttu-id="0b84f-150">所有與並行語彙基元資料行共用資料表的實體，都必須將其對應到屬性</span><span class="sxs-lookup"><span data-stu-id="0b84f-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="0b84f-151">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-151">Low</span></span>      |
| [<span data-ttu-id="0b84f-152">未對應類型的繼承屬性，現在會對應到所有衍生類型的單一資料行</span><span class="sxs-lookup"><span data-stu-id="0b84f-152">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="0b84f-153">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-153">Low</span></span>      |
| [<span data-ttu-id="0b84f-154">外部索引鍵屬性慣例不會再比對與主體屬性相同的名稱</span><span class="sxs-lookup"><span data-stu-id="0b84f-154">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="0b84f-155">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-155">Low</span></span>      |
| [<span data-ttu-id="0b84f-156">如果在 TransactionScope 完成之前未再使用，資料庫連線現在會關閉</span><span class="sxs-lookup"><span data-stu-id="0b84f-156">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="0b84f-157">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-157">Low</span></span>      |
| [<span data-ttu-id="0b84f-158">根據預設，會使用支援欄位</span><span class="sxs-lookup"><span data-stu-id="0b84f-158">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="0b84f-159">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-159">Low</span></span>      |
| [<span data-ttu-id="0b84f-160">找到多個相容的支援欄位時擲回</span><span class="sxs-lookup"><span data-stu-id="0b84f-160">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="0b84f-161">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-161">Low</span></span>      |
| [<span data-ttu-id="0b84f-162">僅欄位的屬性名稱應與欄位名稱相符</span><span class="sxs-lookup"><span data-stu-id="0b84f-162">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="0b84f-163">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-163">Low</span></span>      |
| [<span data-ttu-id="0b84f-164">AddDbContext/AddDbContextPool 再也不會呼叫 AddLogging 與 AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="0b84f-164">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="0b84f-165">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-165">Low</span></span>      |
| [<span data-ttu-id="0b84f-166">DbContext.Entry 現在會執行本機 DetectChanges</span><span class="sxs-lookup"><span data-stu-id="0b84f-166">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="0b84f-167">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-167">Low</span></span>      |
| [<span data-ttu-id="0b84f-168">根據預設，字串和位元組陣列索引鍵不會由用戶端產生</span><span class="sxs-lookup"><span data-stu-id="0b84f-168">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="0b84f-169">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-169">Low</span></span>      |
| [<span data-ttu-id="0b84f-170">ILoggerFactory 現在是限定範圍的服務</span><span class="sxs-lookup"><span data-stu-id="0b84f-170">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="0b84f-171">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-171">Low</span></span>      |
| [<span data-ttu-id="0b84f-172">消極式載入 Proxy 不再假設導覽屬性已完全載入</span><span class="sxs-lookup"><span data-stu-id="0b84f-172">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="0b84f-173">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-173">Low</span></span>      |
| [<span data-ttu-id="0b84f-174">現在根據預設，過度建立內部服務提供者會是錯誤</span><span class="sxs-lookup"><span data-stu-id="0b84f-174">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="0b84f-175">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-175">Low</span></span>      |
| [<span data-ttu-id="0b84f-176">使用單一字串呼叫的 HasOne/HasMany 新行為</span><span class="sxs-lookup"><span data-stu-id="0b84f-176">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="0b84f-177">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-177">Low</span></span>      |
| [<span data-ttu-id="0b84f-178">數個非同步方法的傳回型別已從 Task 變更為 ValueTask</span><span class="sxs-lookup"><span data-stu-id="0b84f-178">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="0b84f-179">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-179">Low</span></span>      |
| [<span data-ttu-id="0b84f-180">Relational:TypeMapping 註解現在變成只有 TypeMapping</span><span class="sxs-lookup"><span data-stu-id="0b84f-180">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="0b84f-181">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-181">Low</span></span>      |
| [<span data-ttu-id="0b84f-182">衍生類型上的 ToTable 會擲回例外狀況</span><span class="sxs-lookup"><span data-stu-id="0b84f-182">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="0b84f-183">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-183">Low</span></span>      |
| [<span data-ttu-id="0b84f-184">EF Core 不會再為 SQLite FK 強制傳送 pragma</span><span class="sxs-lookup"><span data-stu-id="0b84f-184">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="0b84f-185">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-185">Low</span></span>      |
| [<span data-ttu-id="0b84f-186">Microsoft.EntityFrameworkCore.Sqlite 現在相依於 SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="0b84f-186">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="0b84f-187">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-187">Low</span></span>      |
| [<span data-ttu-id="0b84f-188">GUID 值現在於 SQLite 上的儲存形式為 TEXT</span><span class="sxs-lookup"><span data-stu-id="0b84f-188">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="0b84f-189">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-189">Low</span></span>      |
| [<span data-ttu-id="0b84f-190">Char 值現在於 SQLite 上的儲存形式為 TEXT</span><span class="sxs-lookup"><span data-stu-id="0b84f-190">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="0b84f-191">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-191">Low</span></span>      |
| [<span data-ttu-id="0b84f-192">移轉識別碼現在會使用不因文化特性而異的行事曆來產生</span><span class="sxs-lookup"><span data-stu-id="0b84f-192">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="0b84f-193">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-193">Low</span></span>      |
| [<span data-ttu-id="0b84f-194">已從 IDbContextOptionsExtension 移除延伸模組資訊/中繼資料</span><span class="sxs-lookup"><span data-stu-id="0b84f-194">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="0b84f-195">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-195">Low</span></span>      |
| [<span data-ttu-id="0b84f-196">已為 LogQueryPossibleExceptionWithAggregateOperator 重新命名</span><span class="sxs-lookup"><span data-stu-id="0b84f-196">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="0b84f-197">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-197">Low</span></span>      |
| [<span data-ttu-id="0b84f-198">讓 API 的外部索引鍵限制式名稱更清楚</span><span class="sxs-lookup"><span data-stu-id="0b84f-198">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="0b84f-199">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-199">Low</span></span>      |
| [<span data-ttu-id="0b84f-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync 已設為公用</span><span class="sxs-lookup"><span data-stu-id="0b84f-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="0b84f-201">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-201">Low</span></span>      |
| [<span data-ttu-id="0b84f-202">Microsoft.EntityFrameworkCore.Design 現在是 DevelopmentDependency 套件</span><span class="sxs-lookup"><span data-stu-id="0b84f-202">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="0b84f-203">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-203">Low</span></span>      |
| [<span data-ttu-id="0b84f-204">SQLitePCL.raw 已更新為 2.0.0 版</span><span class="sxs-lookup"><span data-stu-id="0b84f-204">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="0b84f-205">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-205">Low</span></span>      |
| [<span data-ttu-id="0b84f-206">NetTopologySuite 已更新為 2.0.0 版</span><span class="sxs-lookup"><span data-stu-id="0b84f-206">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="0b84f-207">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-207">Low</span></span>      |
| [<span data-ttu-id="0b84f-208">必須設定多個不明確的自我參考關聯性</span><span class="sxs-lookup"><span data-stu-id="0b84f-208">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="0b84f-209">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-209">Low</span></span>      |
| [<span data-ttu-id="0b84f-210">DbFunction。架構為 null 或空字串，將其設定為模型的預設架構</span><span class="sxs-lookup"><span data-stu-id="0b84f-210">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="0b84f-211">低</span><span class="sxs-lookup"><span data-stu-id="0b84f-211">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="0b84f-212">不會再於用戶端評估 LINQ 查詢</span><span class="sxs-lookup"><span data-stu-id="0b84f-212">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="0b84f-213">[追蹤問題 #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[另請參閱問題 #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="0b84f-213">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="0b84f-214">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-214">**Old behavior**</span></span>

<span data-ttu-id="0b84f-215">3\.0 以前，在 EF Core 無法將屬於查詢的運算式轉換成 SQL 或參數時，它會自動在用戶端評估運算式。</span><span class="sxs-lookup"><span data-stu-id="0b84f-215">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="0b84f-216">根據預設，對可能相當耗費資源的運算式進行用戶端評估只會觸發警告。</span><span class="sxs-lookup"><span data-stu-id="0b84f-216">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="0b84f-217">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-217">**New behavior**</span></span>

<span data-ttu-id="0b84f-218">從 3.0 開始，EF Core 只允許在用戶端評估最上層投影的運算式 (查詢中的最後一個 `Select()` 呼叫)。</span><span class="sxs-lookup"><span data-stu-id="0b84f-218">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="0b84f-219">當其他查詢部分中的運算式無法轉換成 SQL 或參數時，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0b84f-219">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="0b84f-220">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-220">**Why**</span></span>

<span data-ttu-id="0b84f-221">查詢的自動用戶端評估可執行許多查詢，即使無法轉譯查詢的重要部分也一樣。</span><span class="sxs-lookup"><span data-stu-id="0b84f-221">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="0b84f-222">此行為可能會導致非預期且可能造成傷害的行為，而且該行為可能只會出現在生產環境中。</span><span class="sxs-lookup"><span data-stu-id="0b84f-222">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="0b84f-223">例如，`Where()` 呼叫中無法轉譯的條件可能會導致資料表中的所有資料列從資料庫伺服器移轉，並在用戶端套用篩選。</span><span class="sxs-lookup"><span data-stu-id="0b84f-223">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="0b84f-224">如果資料表只包含幾個開發中的資料列，此情況可能很容易未被察覺；但當應用程式移至生產環境時，由於資料表可能包含數百萬個資料列，因此影響會很大。</span><span class="sxs-lookup"><span data-stu-id="0b84f-224">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="0b84f-225">用戶端評估警告也證明很容易在開發期間遭到忽略。</span><span class="sxs-lookup"><span data-stu-id="0b84f-225">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="0b84f-226">此外，自動用戶端評估可能會導致改善特定運算式的查詢轉譯造成版本間非預期的中斷性變更問題。</span><span class="sxs-lookup"><span data-stu-id="0b84f-226">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="0b84f-227">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-227">**Mitigations**</span></span>

<span data-ttu-id="0b84f-228">如果無法完整轉譯查詢，請以可轉譯的格式來重寫查詢，或是使用 `AsEnumerable()`、`ToList()` 或類似函數來明確將資料帶回用戶端，以便接著使用 LINQ-to-Objects 加以處理。</span><span class="sxs-lookup"><span data-stu-id="0b84f-228">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="0b84f-229">EF Core 3.0 以 .NET Standard 2.1 為目標，而非以 .NET Standard 2.0 為目標</span><span class="sxs-lookup"><span data-stu-id="0b84f-229">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="0b84f-230">追蹤問題 #15498</span><span class="sxs-lookup"><span data-stu-id="0b84f-230">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="0b84f-231">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-231">**Old behavior**</span></span>

<span data-ttu-id="0b84f-232">在 3.0 之前，EF Core 以 .NET Standard 2.0 為目標，且執行於支援該標準的所有平台上，包括 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="0b84f-232">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="0b84f-233">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-233">**New behavior**</span></span>

<span data-ttu-id="0b84f-234">從 3.0 開始，EF Core 以 .NET Standard 2.1 為目標，且執行於支援此標準的所有平台上。</span><span class="sxs-lookup"><span data-stu-id="0b84f-234">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="0b84f-235">這不包括 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="0b84f-235">This does not include .NET Framework.</span></span>

<span data-ttu-id="0b84f-236">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-236">**Why**</span></span>

<span data-ttu-id="0b84f-237">此為跨 .NET 技術的策略性決策之一部分，著重於 .NET Core 與其他現代化 .NET 平台 (例如 Xamarin) 的能力。</span><span class="sxs-lookup"><span data-stu-id="0b84f-237">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="0b84f-238">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-238">**Mitigations**</span></span>

<span data-ttu-id="0b84f-239">請考慮移至現代化的 .NET 平台。</span><span class="sxs-lookup"><span data-stu-id="0b84f-239">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="0b84f-240">如果無法如此做，請繼續使用 EF Core 2.1 或 EF Core 2.2，這兩者皆支援 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="0b84f-240">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="0b84f-241">Entity Framework Core 不再屬於 ASP.NET Core 共用架構</span><span class="sxs-lookup"><span data-stu-id="0b84f-241">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="0b84f-242">追蹤問題 Announcements#325</span><span class="sxs-lookup"><span data-stu-id="0b84f-242">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="0b84f-243">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-243">**Old behavior**</span></span>

<span data-ttu-id="0b84f-244">在 ASP.NET Core 3.0 以前，當您新增 `Microsoft.AspNetCore.App` 或 `Microsoft.AspNetCore.All` 的套件參考時，它會包含 EF Core 及部分 EF Core 資料提供者 (例如 SQL Server 提供者)。</span><span class="sxs-lookup"><span data-stu-id="0b84f-244">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="0b84f-245">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-245">**New behavior**</span></span>

<span data-ttu-id="0b84f-246">從 3.0 開始，ASP.NET Core 共用架構不會包含 EF Core 或任何 EF Core 資料提供者。</span><span class="sxs-lookup"><span data-stu-id="0b84f-246">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="0b84f-247">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-247">**Why**</span></span>

<span data-ttu-id="0b84f-248">在這項變更之前，取得 EF Core 會根據應用程式是否以 ASP.NET Core 和 SQL Server 為目標而需要不同的步驟。</span><span class="sxs-lookup"><span data-stu-id="0b84f-248">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="0b84f-249">此外，升級 ASP.NET Core 會強制升級 EF Core 和 SQL Server 提供者，這不一定符合需求。</span><span class="sxs-lookup"><span data-stu-id="0b84f-249">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="0b84f-250">透過這項變更，取得 EF Core 的體驗對所有提供者、支援的 .NET 實作和應用程式類型都相同。</span><span class="sxs-lookup"><span data-stu-id="0b84f-250">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="0b84f-251">開發人員現在也可以精確控制何時升級 EF Core 和 EF Core 資料提供者。</span><span class="sxs-lookup"><span data-stu-id="0b84f-251">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="0b84f-252">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-252">**Mitigations**</span></span>

<span data-ttu-id="0b84f-253">若要在 ASP.NET Core 3.0 應用程式或其他支援的應用程式中使用 EF Core，請明確將套件參考加入應用程式會使用的 EF Core 資料庫提供者。</span><span class="sxs-lookup"><span data-stu-id="0b84f-253">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="0b84f-254">EF Core 命令列工具 dotnet ef 不再是 .NET Core SDK 的一部分</span><span class="sxs-lookup"><span data-stu-id="0b84f-254">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="0b84f-255">追蹤問題 #14016</span><span class="sxs-lookup"><span data-stu-id="0b84f-255">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="0b84f-256">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-256">**Old behavior**</span></span>

<span data-ttu-id="0b84f-257">在 3.0 之前，`dotnet ef` 工具包含在 .NET Core SDK，並可以輕易地從任何專案的命令列使用，而不需要額外步驟。</span><span class="sxs-lookup"><span data-stu-id="0b84f-257">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="0b84f-258">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-258">**New behavior**</span></span>

<span data-ttu-id="0b84f-259">從 3.0 開始，.NET SDK 不包含 `dotnet ef` 工具，因此您必須明確地將它安裝為本機或全域工具才能使用。</span><span class="sxs-lookup"><span data-stu-id="0b84f-259">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="0b84f-260">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-260">**Why**</span></span>

<span data-ttu-id="0b84f-261">這項變更可讓我們將 `dotnet ef` 當作 NuGet 上一般的 .NET CLI 工具來散發和更新，這點與 EF Core 3.0 一律當作 NuGet 套件散發的事實一致。</span><span class="sxs-lookup"><span data-stu-id="0b84f-261">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="0b84f-262">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-262">**Mitigations**</span></span>

<span data-ttu-id="0b84f-263">若要能夠管理移轉或支撐 `DbContext`，請安裝 `dotnet-ef` 作為全域工具：</span><span class="sxs-lookup"><span data-stu-id="0b84f-263">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="0b84f-264">您也可以在還原專案相依性時取得它作為本機工具 (該專案是使用[工具資訊清單檔](https://github.com/dotnet/cli/issues/10288)將它宣告為工具相依性)。</span><span class="sxs-lookup"><span data-stu-id="0b84f-264">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="0b84f-265">FromSql、ExecuteSql 和 ExecuteSqlAsync 已重新命名</span><span class="sxs-lookup"><span data-stu-id="0b84f-265">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="0b84f-266">追蹤問題 #10996</span><span class="sxs-lookup"><span data-stu-id="0b84f-266">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="0b84f-267">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-267">**Old behavior**</span></span>

<span data-ttu-id="0b84f-268">在 EF Core 3.0 之前，這些方法名稱已多載以使用一般字串，或應插入至 SQL 和參數的字串。</span><span class="sxs-lookup"><span data-stu-id="0b84f-268">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="0b84f-269">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-269">**New behavior**</span></span>

<span data-ttu-id="0b84f-270">從 EF Core 3.0 開始，請使用 `FromSqlRaw`、`ExecuteSqlRaw` 和 `ExecuteSqlRawAsync` 建立參數化查詢，其中參數會分別從查詢字串傳遞。</span><span class="sxs-lookup"><span data-stu-id="0b84f-270">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="0b84f-271">例如:</span><span class="sxs-lookup"><span data-stu-id="0b84f-271">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="0b84f-272">使用 `FromSqlInterpolated`、`ExecuteSqlInterpolated` 和 `ExecuteSqlInterpolatedAsync` 建立參數化查詢，其中參數會作為插入查詢字串的一部分傳回。</span><span class="sxs-lookup"><span data-stu-id="0b84f-272">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="0b84f-273">例如:</span><span class="sxs-lookup"><span data-stu-id="0b84f-273">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="0b84f-274">請注意，上述兩個查詢都會產生具有相同 SQL 參數的相同參數化 SQL。</span><span class="sxs-lookup"><span data-stu-id="0b84f-274">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="0b84f-275">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-275">**Why**</span></span>

<span data-ttu-id="0b84f-276">像這樣的方法多載，使得原本要呼叫插入字串方法很容易意外呼叫原始字串方法，或反之。</span><span class="sxs-lookup"><span data-stu-id="0b84f-276">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="0b84f-277">這可能會導致查詢在應該參數化時不進行參數化。</span><span class="sxs-lookup"><span data-stu-id="0b84f-277">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="0b84f-278">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-278">**Mitigations**</span></span>

<span data-ttu-id="0b84f-279">切換至使用新的方法名稱。</span><span class="sxs-lookup"><span data-stu-id="0b84f-279">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="0b84f-280">無法撰寫與預存程式搭配使用時的 FromSql 方法</span><span class="sxs-lookup"><span data-stu-id="0b84f-280">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="0b84f-281">追蹤問題 #15392</span><span class="sxs-lookup"><span data-stu-id="0b84f-281">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="0b84f-282">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-282">**Old behavior**</span></span>

<span data-ttu-id="0b84f-283">在 EF Core 3.0 之前，FromSql 方法會嘗試偵測是否可以根據傳遞的 SQL 來進行撰寫。</span><span class="sxs-lookup"><span data-stu-id="0b84f-283">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="0b84f-284">當 SQL 不是可組合的，如同預存程式，它會進行用戶端評估。</span><span class="sxs-lookup"><span data-stu-id="0b84f-284">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="0b84f-285">下列查詢的運作方式是在伺服器上執行預存程式，並在用戶端上進行 FirstOrDefault。</span><span class="sxs-lookup"><span data-stu-id="0b84f-285">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="0b84f-286">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-286">**New behavior**</span></span>

<span data-ttu-id="0b84f-287">從 EF Core 3.0 開始，EF Core 不會嘗試剖析 SQL。</span><span class="sxs-lookup"><span data-stu-id="0b84f-287">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="0b84f-288">因此，如果您在 FromSqlRaw/FromSqlInterpolated 之後撰寫，則 EF Core 會藉由導致子查詢來撰寫 SQL。</span><span class="sxs-lookup"><span data-stu-id="0b84f-288">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="0b84f-289">因此，如果您使用具有組合的預存程式，則會收到無效 SQL 語法的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0b84f-289">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="0b84f-290">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-290">**Why**</span></span>

<span data-ttu-id="0b84f-291">EF Core 3.0 不支援自動用戶端評估，因為它容易發生錯誤，如[這裡](#linq-queries-are-no-longer-evaluated-on-the-client)所述。</span><span class="sxs-lookup"><span data-stu-id="0b84f-291">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="0b84f-292">**緩解**</span><span class="sxs-lookup"><span data-stu-id="0b84f-292">**Mitigation**</span></span>

<span data-ttu-id="0b84f-293">如果您使用 FromSqlRaw/FromSqlInterpolated 中的預存程式，您就知道它無法由撰寫，因此您可以在 FromSql 方法呼叫之後加入__enumerable.asenumerable/AsAsyncEnumerable__ ，以避免伺服器端上的任何組合。</span><span class="sxs-lookup"><span data-stu-id="0b84f-293">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="0b84f-294">FromSql 方法只能在查詢根目錄上指定</span><span class="sxs-lookup"><span data-stu-id="0b84f-294">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="0b84f-295">追蹤問題 #15704</span><span class="sxs-lookup"><span data-stu-id="0b84f-295">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="0b84f-296">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-296">**Old behavior**</span></span>

<span data-ttu-id="0b84f-297">在 EF Core 3.0 之前，可以在查詢中的任何位置指定 `FromSql` 方法。</span><span class="sxs-lookup"><span data-stu-id="0b84f-297">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="0b84f-298">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-298">**New behavior**</span></span>

<span data-ttu-id="0b84f-299">從 EF Core 3.0 開始，新的 `FromSqlRaw` 與 `FromSqlInterpolated` 方法 (取代 `FromSql`) 只能在查詢根目錄上指定，亦即直接在 `DbSet<>` 上指定。</span><span class="sxs-lookup"><span data-stu-id="0b84f-299">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="0b84f-300">嘗試在其他任何位置指定它們，將會導致編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="0b84f-300">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="0b84f-301">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-301">**Why**</span></span>

<span data-ttu-id="0b84f-302">在 `DbSet` 以外的任何地方指定 `FromSql` 沒有新增的意義或附加價值，而且在某些情況下可能會導致模稜兩可。</span><span class="sxs-lookup"><span data-stu-id="0b84f-302">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="0b84f-303">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-303">**Mitigations**</span></span>

<span data-ttu-id="0b84f-304">`FromSql` 引動過程應該直接移至它們適用的 `DbSet`。</span><span class="sxs-lookup"><span data-stu-id="0b84f-304">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="0b84f-305">無追蹤查詢已不再執行身分識別解析</span><span class="sxs-lookup"><span data-stu-id="0b84f-305">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="0b84f-306">追蹤問題 #13518</span><span class="sxs-lookup"><span data-stu-id="0b84f-306">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="0b84f-307">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-307">**Old behavior**</span></span>

<span data-ttu-id="0b84f-308">在 EF Core 3.0 之前，每次出現具有給定類型與識別碼的實體時，皆會使用相同的實體執行個體。</span><span class="sxs-lookup"><span data-stu-id="0b84f-308">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="0b84f-309">如此符合追蹤查詢的行為。</span><span class="sxs-lookup"><span data-stu-id="0b84f-309">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="0b84f-310">例如，下列查詢：</span><span class="sxs-lookup"><span data-stu-id="0b84f-310">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="0b84f-311">會為每個與給定類別相關聯的 `Product`，傳回相同的 `Category` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="0b84f-311">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="0b84f-312">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-312">**New behavior**</span></span>

<span data-ttu-id="0b84f-313">從 EF Core 3.0 開始，當具有給定類型與識別碼的實體，出現在傳回圖形的不同位置時，將會建立不同的實體執行個體。</span><span class="sxs-lookup"><span data-stu-id="0b84f-313">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="0b84f-314">例如，即使當兩個產品與相同的類別相關聯，上述查詢現在會為每個 `Category` 傳回新的 `Product` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="0b84f-314">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="0b84f-315">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-315">**Why**</span></span>

<span data-ttu-id="0b84f-316">身分識別解析 (也就是，決定實體與之前所發生的實體具有相同的類型與識別碼) 會加入額外的效能與記憶體負荷。</span><span class="sxs-lookup"><span data-stu-id="0b84f-316">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="0b84f-317">這通常會執行為何一開始就使用無追蹤查詢的計數器。</span><span class="sxs-lookup"><span data-stu-id="0b84f-317">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="0b84f-318">此外，雖然身分識別解析有時非常有用，但若實體要序列化並會傳送給用戶端 (對無追蹤查詢而言很常見)，則不需要。</span><span class="sxs-lookup"><span data-stu-id="0b84f-318">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="0b84f-319">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-319">**Mitigations**</span></span>

<span data-ttu-id="0b84f-320">若需要身分識別解析，請使用追蹤查詢。</span><span class="sxs-lookup"><span data-stu-id="0b84f-320">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="0b84f-321">~~查詢執行會在偵錯層級記錄~~已還原</span><span class="sxs-lookup"><span data-stu-id="0b84f-321">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="0b84f-322">追蹤問題 #14523</span><span class="sxs-lookup"><span data-stu-id="0b84f-322">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="0b84f-323">我們還原此變更的原因是 EF Core 3.0 中的新設定允許應用程式指定任何事件的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="0b84f-323">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="0b84f-324">例如，若要將 SQL 的記錄切換到 `Debug`，請明確地在 `OnConfiguring` 或 `AddDbContext` 中設定層級：</span><span class="sxs-lookup"><span data-stu-id="0b84f-324">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="0b84f-325">實體執行個體上不會再設定暫存索引鍵值</span><span class="sxs-lookup"><span data-stu-id="0b84f-325">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="0b84f-326">追蹤問題 #12378</span><span class="sxs-lookup"><span data-stu-id="0b84f-326">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="0b84f-327">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-327">**Old behavior**</span></span>

<span data-ttu-id="0b84f-328">在 EF Core 3.0 以前，會對所有索引鍵屬性指派暫存值，這些屬性稍後會有資料庫產生的實值。</span><span class="sxs-lookup"><span data-stu-id="0b84f-328">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="0b84f-329">這些暫存值通常是龐大的負值。</span><span class="sxs-lookup"><span data-stu-id="0b84f-329">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="0b84f-330">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-330">**New behavior**</span></span>

<span data-ttu-id="0b84f-331">從 3.0 開始，EF Core 會將暫存索引鍵值儲存為實體追蹤資訊的一部分，至於索引鍵屬性本身則保持不變。</span><span class="sxs-lookup"><span data-stu-id="0b84f-331">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="0b84f-332">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-332">**Why**</span></span>

<span data-ttu-id="0b84f-333">這項變更的目的是為了防止在將某個 `DbContext` 執行個體先前追蹤的實體移至不同的 `DbContext` 執行個體時，錯誤地把暫存索引鍵值變成永久值。</span><span class="sxs-lookup"><span data-stu-id="0b84f-333">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="0b84f-334">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-334">**Mitigations**</span></span>

<span data-ttu-id="0b84f-335">若應用程式會將主索引鍵指派給外部索引鍵以形成實體間關聯，則可能會在主索引鍵是由存放區產生並屬於 `Added` 狀態的實體時採用舊行為。</span><span class="sxs-lookup"><span data-stu-id="0b84f-335">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="0b84f-336">這可透過下列方式來避免：</span><span class="sxs-lookup"><span data-stu-id="0b84f-336">This can be avoided by:</span></span>
* <span data-ttu-id="0b84f-337">不使用存放區產生的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="0b84f-337">Not using store-generated keys.</span></span>
* <span data-ttu-id="0b84f-338">設定導覽屬性以形成關聯性，而不是設定外部索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="0b84f-338">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="0b84f-339">從實體的追蹤資訊取得實際暫存索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="0b84f-339">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="0b84f-340">例如，`context.Entry(blog).Property(e => e.Id).CurrentValue` 會傳回暫存值，即使尚未設定 `blog.Id` 本身也一樣。</span><span class="sxs-lookup"><span data-stu-id="0b84f-340">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="0b84f-341">DetectChanges 接受存放區產生的索引鍵值</span><span class="sxs-lookup"><span data-stu-id="0b84f-341">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="0b84f-342">追蹤問題 #14616</span><span class="sxs-lookup"><span data-stu-id="0b84f-342">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="0b84f-343">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-343">**Old behavior**</span></span>

<span data-ttu-id="0b84f-344">在 EF Core 3.0 以前，`DetectChanges` 所發現未被追蹤的實體會以 `Added` 狀態追蹤，並在呼叫 `SaveChanges` 時以新的資料列插入。</span><span class="sxs-lookup"><span data-stu-id="0b84f-344">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="0b84f-345">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-345">**New behavior**</span></span>

<span data-ttu-id="0b84f-346">從 EF Core 3.0 開始，如果實體使用產生的索引鍵值並已設定一些索引鍵值，則實體會以 `Modified` 狀態追蹤。</span><span class="sxs-lookup"><span data-stu-id="0b84f-346">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="0b84f-347">這表示實體的資料列假設存在，而且會在呼叫 `SaveChanges` 時更新。</span><span class="sxs-lookup"><span data-stu-id="0b84f-347">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="0b84f-348">如果未設定索引鍵值，或者如果實體類型未使用產生的索引鍵，則新的實體仍會如同舊版以 `Added` 追蹤。</span><span class="sxs-lookup"><span data-stu-id="0b84f-348">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="0b84f-349">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-349">**Why**</span></span>

<span data-ttu-id="0b84f-350">這項變更的目的是為了更輕鬆一致地使用中斷連接的實體圖形，同時使用存放區產生的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="0b84f-350">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="0b84f-351">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-351">**Mitigations**</span></span>

<span data-ttu-id="0b84f-352">如果實體類型已設定為使用產生的索引鍵，但針對新的執行個體明確設定了索引鍵值，這項變更可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="0b84f-352">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="0b84f-353">修正方法是明確設定索引鍵屬性不使用產生的值。</span><span class="sxs-lookup"><span data-stu-id="0b84f-353">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="0b84f-354">例如，使用 Fluent API：</span><span class="sxs-lookup"><span data-stu-id="0b84f-354">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="0b84f-355">或者，使用資料註解：</span><span class="sxs-lookup"><span data-stu-id="0b84f-355">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="0b84f-356">現在預設會立即發生串聯刪除</span><span class="sxs-lookup"><span data-stu-id="0b84f-356">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="0b84f-357">追蹤問題 #10114</span><span class="sxs-lookup"><span data-stu-id="0b84f-357">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="0b84f-358">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-358">**Old behavior**</span></span>

<span data-ttu-id="0b84f-359">在 3.0 以前，除非呼叫 SaveChanges，否則 EF Core 不會套用串聯動作 (刪除必要主體或提供必要主體關聯性時刪除相依實體)。</span><span class="sxs-lookup"><span data-stu-id="0b84f-359">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="0b84f-360">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-360">**New behavior**</span></span>

<span data-ttu-id="0b84f-361">從 3.0 開始，EF Core 會在偵測到觸發條件時立即套用串聯動作。</span><span class="sxs-lookup"><span data-stu-id="0b84f-361">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="0b84f-362">例如，呼叫 `context.Remove()` 刪除主要實體會導致所有追蹤的相關必要相依項目也會立即設定為 `Deleted`。</span><span class="sxs-lookup"><span data-stu-id="0b84f-362">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="0b84f-363">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-363">**Why**</span></span>

<span data-ttu-id="0b84f-364">這項變更的目的是為了改善資料繫結和稽核情節的體驗，在這些情節中了解呼叫 `SaveChanges`「之前」將刪除哪些實體是很重要的。</span><span class="sxs-lookup"><span data-stu-id="0b84f-364">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="0b84f-365">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-365">**Mitigations**</span></span>

<span data-ttu-id="0b84f-366">透過設定 `context.ChangedTracker` 可以還原舊行為。</span><span class="sxs-lookup"><span data-stu-id="0b84f-366">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="0b84f-367">例如:</span><span class="sxs-lookup"><span data-stu-id="0b84f-367">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="0b84f-368">相關實體的積極式載入現在會出現在單一查詢中</span><span class="sxs-lookup"><span data-stu-id="0b84f-368">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="0b84f-369">追蹤問題 #18022</span><span class="sxs-lookup"><span data-stu-id="0b84f-369">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="0b84f-370">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-370">**Old behavior**</span></span>

<span data-ttu-id="0b84f-371">在3.0 之前，立即透過 @no__t 0 運算子載入集合導覽，會導致關係資料庫產生多個查詢，每個相關實體類型各一個。</span><span class="sxs-lookup"><span data-stu-id="0b84f-371">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="0b84f-372">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-372">**New behavior**</span></span>

<span data-ttu-id="0b84f-373">從3.0 開始，EF Core 會在關係資料庫上產生具有聯結的單一查詢。</span><span class="sxs-lookup"><span data-stu-id="0b84f-373">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="0b84f-374">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-374">**Why**</span></span>

<span data-ttu-id="0b84f-375">發出多個查詢來執行單一 LINQ 查詢，會造成許多問題，包括負面效能，因為需要多個資料庫往返，而當每個查詢可能觀察到資料庫的不同狀態時，就會發生資料一致性問題。</span><span class="sxs-lookup"><span data-stu-id="0b84f-375">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="0b84f-376">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-376">**Mitigations**</span></span>

<span data-ttu-id="0b84f-377">雖然技術上來說這不是一項重大變更，但當單一查詢在集合導覽上包含大量的 @no__t 0 運算子時，可能會對應用程式效能造成相當大的影響。</span><span class="sxs-lookup"><span data-stu-id="0b84f-377">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="0b84f-378">如需詳細資訊和以更有效率的方式重寫查詢，[請參閱此批註](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-537219137)。</span><span class="sxs-lookup"><span data-stu-id="0b84f-378">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-537219137) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="0b84f-379">DeleteBehavior.Restrict 具有更簡潔的語意</span><span class="sxs-lookup"><span data-stu-id="0b84f-379">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="0b84f-380">追蹤問題 #12661</span><span class="sxs-lookup"><span data-stu-id="0b84f-380">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="0b84f-381">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-381">**Old behavior**</span></span>

<span data-ttu-id="0b84f-382">在 3.0 以前，`DeleteBehavior.Restrict` 使用 `Restrict` 語意在資料庫中建立外部索引鍵，但也以不明顯的方式變更內部修復。</span><span class="sxs-lookup"><span data-stu-id="0b84f-382">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="0b84f-383">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-383">**New behavior**</span></span>

<span data-ttu-id="0b84f-384">從 3.0 開始，`DeleteBehavior.Restrict` 會確保外部索引鍵使用 `Restrict` 語意來建立 (也就是不會有重疊顯示，拋出條件約束違規)，不會影響 EF 內部修復。</span><span class="sxs-lookup"><span data-stu-id="0b84f-384">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="0b84f-385">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-385">**Why**</span></span>

<span data-ttu-id="0b84f-386">這項變更可藉由直覺方式提升使用 `DeleteBehavior` 的體驗，而不會發生非預期的副作用。</span><span class="sxs-lookup"><span data-stu-id="0b84f-386">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="0b84f-387">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-387">**Mitigations**</span></span>

<span data-ttu-id="0b84f-388">使用 `DeleteBehavior.ClientNoAction` 可以還原舊行為。</span><span class="sxs-lookup"><span data-stu-id="0b84f-388">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="0b84f-389">查詢類型會與實體類型合併</span><span class="sxs-lookup"><span data-stu-id="0b84f-389">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="0b84f-390">追蹤問題 #14194</span><span class="sxs-lookup"><span data-stu-id="0b84f-390">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="0b84f-391">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-391">**Old behavior**</span></span>

<span data-ttu-id="0b84f-392">在 EF Core 3.0 以前，[查詢類型](xref:core/modeling/keyless-entity-types)可讓您查詢未以結構化方式定義主索引鍵的資料。</span><span class="sxs-lookup"><span data-stu-id="0b84f-392">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="0b84f-393">換句話說，查詢類型是用於對應沒有索引鍵的實體類型 (較有可能來自檢視，但也有可能來自資料表)，而一般實體類型是用於索引鍵可供使用時 (較有可能來自資料表，但也有可能來自檢視)。</span><span class="sxs-lookup"><span data-stu-id="0b84f-393">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="0b84f-394">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-394">**New behavior**</span></span>

<span data-ttu-id="0b84f-395">查詢類型現在會成為沒有主索引鍵的實體類型。</span><span class="sxs-lookup"><span data-stu-id="0b84f-395">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="0b84f-396">無索引鍵的實體類型功能與舊版查詢類型相同。</span><span class="sxs-lookup"><span data-stu-id="0b84f-396">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="0b84f-397">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-397">**Why**</span></span>

<span data-ttu-id="0b84f-398">這項變更的目的是為了降低查詢類型用途的混淆。</span><span class="sxs-lookup"><span data-stu-id="0b84f-398">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="0b84f-399">具體來說，它們是無索引鍵的實體類型，因此本質上是唯讀的，但不應該只因為實體類型必須是唯讀就使用。</span><span class="sxs-lookup"><span data-stu-id="0b84f-399">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="0b84f-400">同樣地，它們通常會對應至檢視，但這只是因為檢視通常未定義索引鍵。</span><span class="sxs-lookup"><span data-stu-id="0b84f-400">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="0b84f-401">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-401">**Mitigations**</span></span>

<span data-ttu-id="0b84f-402">API 的下列組件現已淘汰：</span><span class="sxs-lookup"><span data-stu-id="0b84f-402">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="0b84f-403">**`ModelBuilder.Query<>()`** - 必須改為呼叫 `ModelBuilder.Entity<>().HasNoKey()` 將實體類型標示為沒有索引鍵。</span><span class="sxs-lookup"><span data-stu-id="0b84f-403">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="0b84f-404">為了避免在必須有主索引鍵但不符合慣例時設定錯誤，目前仍未將此設定為慣例。</span><span class="sxs-lookup"><span data-stu-id="0b84f-404">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="0b84f-405">**`DbQuery<>`** - 應改用 `DbSet<>`。</span><span class="sxs-lookup"><span data-stu-id="0b84f-405">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="0b84f-406">**`DbContext.Query<>()`** - 應改用 `DbContext.Set<>()`。</span><span class="sxs-lookup"><span data-stu-id="0b84f-406">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="0b84f-407">自有類型關聯性的設定 API 已變更</span><span class="sxs-lookup"><span data-stu-id="0b84f-407">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="0b84f-408">[追蹤問題 #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[追蹤問題 #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[追蹤問題 #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="0b84f-408">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="0b84f-409">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-409">**Old behavior**</span></span>

<span data-ttu-id="0b84f-410">在 EF Core 3.0 以前，會在呼叫 `OwnsOne` 或 `OwnsMany` 之後直接執行自有關聯性的設定。</span><span class="sxs-lookup"><span data-stu-id="0b84f-410">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="0b84f-411">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-411">**New behavior**</span></span>

<span data-ttu-id="0b84f-412">從 EF Core 3.0 開始，現在會有 Fluent API 使用 `WithOwner()` 將導覽屬性設定為擁有者。</span><span class="sxs-lookup"><span data-stu-id="0b84f-412">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="0b84f-413">例如:</span><span class="sxs-lookup"><span data-stu-id="0b84f-413">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="0b84f-414">擁有者與自有之間關聯性的相關設定現在應該在 `WithOwner()` 之後鏈結，類似於其他關聯性的設定方式。</span><span class="sxs-lookup"><span data-stu-id="0b84f-414">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="0b84f-415">但自有類型本身的設定仍會在 `OwnsOne()/OwnsMany()` 之後鏈結。</span><span class="sxs-lookup"><span data-stu-id="0b84f-415">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="0b84f-416">例如:</span><span class="sxs-lookup"><span data-stu-id="0b84f-416">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

<span data-ttu-id="0b84f-417">此外，使用自有類型目標呼叫 `Entity()`、`HasOne()` 或 `Set()` 現在會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0b84f-417">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="0b84f-418">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-418">**Why**</span></span>

<span data-ttu-id="0b84f-419">這項變更的目的是為了更清楚地劃分設定自有類型本身，以及設定自有類型的「關聯性」。</span><span class="sxs-lookup"><span data-stu-id="0b84f-419">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="0b84f-420">如此可避免 `HasForeignKey` 等方法的模稜兩可和混淆。</span><span class="sxs-lookup"><span data-stu-id="0b84f-420">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="0b84f-421">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-421">**Mitigations**</span></span>

<span data-ttu-id="0b84f-422">將自有類型關聯性的設定變更為使用新的 API 介面，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="0b84f-422">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="0b84f-423">現在可選用以主體來共用資料表的相依實體</span><span class="sxs-lookup"><span data-stu-id="0b84f-423">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="0b84f-424">追蹤問題 #9005</span><span class="sxs-lookup"><span data-stu-id="0b84f-424">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="0b84f-425">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-425">**Old behavior**</span></span>

<span data-ttu-id="0b84f-426">請考慮下列模型：</span><span class="sxs-lookup"><span data-stu-id="0b84f-426">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```
<span data-ttu-id="0b84f-427">在 EF Core 3.0 之前，如果 `OrderDetails` 由 `Order` 擁有，或明確對應至相同的資料表，則在新增新的 `Order` 時一律需要 `OrderDetails` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="0b84f-427">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="0b84f-428">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-428">**New behavior**</span></span>

<span data-ttu-id="0b84f-429">從 3.0 開始，EF 允許新增 `Order` 而不需要 `OrderDetails`，並會對應所有 `OrderDetails` 屬性，除了可為 Null 之資料行的主索引鍵以外。</span><span class="sxs-lookup"><span data-stu-id="0b84f-429">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="0b84f-430">查詢時，如果任何必要的屬性不具有值，或如果其具有主索引鍵以外的不必要屬性，且所有屬性都是 `null`，則 EF Core 會將 `OrderDetails` 設為 `null`。</span><span class="sxs-lookup"><span data-stu-id="0b84f-430">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="0b84f-431">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-431">**Mitigations**</span></span>

<span data-ttu-id="0b84f-432">如果您的模型具有與所有選擇性資料行共用相依資料表，但指向該資料表的導覽不預期為 `null`，則應修改應用程式，以處理當導覽為 `null` 時的情況。</span><span class="sxs-lookup"><span data-stu-id="0b84f-432">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="0b84f-433">如果這不可行，則應將必要屬性新增至實體類型，或至少應有一個屬性指派其非 `null` 的值。</span><span class="sxs-lookup"><span data-stu-id="0b84f-433">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="0b84f-434">共用具有並行語彙基元資料行的所有實體，都必須將其對應至屬性</span><span class="sxs-lookup"><span data-stu-id="0b84f-434">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="0b84f-435">追蹤問題 #14154</span><span class="sxs-lookup"><span data-stu-id="0b84f-435">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="0b84f-436">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-436">**Old behavior**</span></span>

<span data-ttu-id="0b84f-437">請考慮下列模型：</span><span class="sxs-lookup"><span data-stu-id="0b84f-437">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
<span data-ttu-id="0b84f-438">在 EF Core 3.0之前，如果 `OrderDetails` 由 `Order` 擁有，或明確對應至相同資料表，那麼僅更新 `OrderDetails` 將不會更新用戶端上的 `Version` 值，且下一次更新將會失敗。</span><span class="sxs-lookup"><span data-stu-id="0b84f-438">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="0b84f-439">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-439">**New behavior**</span></span>

<span data-ttu-id="0b84f-440">從 3.0 開始，EF Core 會將新的 `Version` 值傳播至 `Order` (如果其擁有 `OrderDetails`)。</span><span class="sxs-lookup"><span data-stu-id="0b84f-440">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="0b84f-441">否則，在模型驗證期間會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0b84f-441">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="0b84f-442">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-442">**Why**</span></span>

<span data-ttu-id="0b84f-443">這項變更的目的，是為了避免在僅更新對應至相同資料表的其中一個實體時，出現過時的並行語彙基元值。</span><span class="sxs-lookup"><span data-stu-id="0b84f-443">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="0b84f-444">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-444">**Mitigations**</span></span>

<span data-ttu-id="0b84f-445">共用資料表的所有實體，都必須包含對應至並行語彙基元資料行的屬性。</span><span class="sxs-lookup"><span data-stu-id="0b84f-445">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="0b84f-446">在陰影狀態中建立一個是可能的：</span><span class="sxs-lookup"><span data-stu-id="0b84f-446">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="0b84f-447">未對應類型的繼承屬性，現在會對應至所有衍生類型的單一資料行</span><span class="sxs-lookup"><span data-stu-id="0b84f-447">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="0b84f-448">追蹤問題 #13998</span><span class="sxs-lookup"><span data-stu-id="0b84f-448">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="0b84f-449">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-449">**Old behavior**</span></span>

<span data-ttu-id="0b84f-450">請考慮下列模型：</span><span class="sxs-lookup"><span data-stu-id="0b84f-450">Consider the following model:</span></span>
```C#
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

<span data-ttu-id="0b84f-451">在 EF Core 3.0 之前，`ShippingAddress` 屬性會根據預設，為 `BulkOrder` 和 `Order` 對應至個別資料行。</span><span class="sxs-lookup"><span data-stu-id="0b84f-451">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="0b84f-452">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-452">**New behavior**</span></span>

<span data-ttu-id="0b84f-453">從 3.0 開始，EF Core 只會為 `ShippingAddress` 建立一個資料行。</span><span class="sxs-lookup"><span data-stu-id="0b84f-453">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="0b84f-454">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-454">**Why**</span></span>

<span data-ttu-id="0b84f-455">舊行為是非預期的。</span><span class="sxs-lookup"><span data-stu-id="0b84f-455">The old behavoir was unexpected.</span></span>

<span data-ttu-id="0b84f-456">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-456">**Mitigations**</span></span>

<span data-ttu-id="0b84f-457">屬性仍可以在衍生類型上明確對應至個別資料行：</span><span class="sxs-lookup"><span data-stu-id="0b84f-457">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

<a name="fkp"></a>

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="0b84f-458">外部索引鍵屬性慣例不會再比對與主體屬性相同的名稱</span><span class="sxs-lookup"><span data-stu-id="0b84f-458">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="0b84f-459">追蹤問題 #13274</span><span class="sxs-lookup"><span data-stu-id="0b84f-459">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="0b84f-460">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-460">**Old behavior**</span></span>

<span data-ttu-id="0b84f-461">請考慮下列模型：</span><span class="sxs-lookup"><span data-stu-id="0b84f-461">Consider the following model:</span></span>
```C#
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```
<span data-ttu-id="0b84f-462">在 EF Core 3.0 以前，`CustomerId` 屬性依照慣例會用於外部索引鍵。</span><span class="sxs-lookup"><span data-stu-id="0b84f-462">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="0b84f-463">不過，如果 `Order` 是自有類型，則這也會將 `CustomerId` 設為主索引鍵，而這通常不符合預期。</span><span class="sxs-lookup"><span data-stu-id="0b84f-463">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="0b84f-464">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-464">**New behavior**</span></span>

<span data-ttu-id="0b84f-465">從 3.0 開始，如果屬性的名稱與主體屬性相同，依照慣例，EF Core 不會嘗試將屬性用於外部索引鍵。</span><span class="sxs-lookup"><span data-stu-id="0b84f-465">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="0b84f-466">但仍會比對與主體屬性名稱串連的主體類型名稱，以及與主體屬性名稱模式串連的導覽名稱。</span><span class="sxs-lookup"><span data-stu-id="0b84f-466">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="0b84f-467">例如:</span><span class="sxs-lookup"><span data-stu-id="0b84f-467">For example:</span></span>

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

<span data-ttu-id="0b84f-468">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-468">**Why**</span></span>

<span data-ttu-id="0b84f-469">這項變更的目的是為了避免錯誤地在自有類型上定義主索引鍵屬性。</span><span class="sxs-lookup"><span data-stu-id="0b84f-469">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="0b84f-470">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-470">**Mitigations**</span></span>

<span data-ttu-id="0b84f-471">如果屬性預定會作為外部索引鍵，並因此成為主索引鍵的一部分，請明確進行這類設定。</span><span class="sxs-lookup"><span data-stu-id="0b84f-471">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="0b84f-472">如果在 TransactionScope 完成之前未再使用，則資料庫連線現在已關閉</span><span class="sxs-lookup"><span data-stu-id="0b84f-472">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="0b84f-473">追蹤問題 #14218</span><span class="sxs-lookup"><span data-stu-id="0b84f-473">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="0b84f-474">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-474">**Old behavior**</span></span>

<span data-ttu-id="0b84f-475">在 EF Core 3.0 之前，如果內容在 `TransactionScope` 內開啟連線，則當目前 `TransactionScope` 處於作用中時，連線將保持開啟。</span><span class="sxs-lookup"><span data-stu-id="0b84f-475">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point
        
        var categories = context.ProductCategories().ToList();
    }
}
```

<span data-ttu-id="0b84f-476">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-476">**New behavior**</span></span>

<span data-ttu-id="0b84f-477">從 3.0 開始，EF Core 在使用完連線後會將其關閉。</span><span class="sxs-lookup"><span data-stu-id="0b84f-477">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="0b84f-478">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-478">**Why**</span></span>

<span data-ttu-id="0b84f-479">此變更允許在相同 `TransactionScope` 中使用多個內容。</span><span class="sxs-lookup"><span data-stu-id="0b84f-479">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="0b84f-480">新的行為也符合 EF6。</span><span class="sxs-lookup"><span data-stu-id="0b84f-480">The new behavior also matches EF6.</span></span>

<span data-ttu-id="0b84f-481">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-481">**Mitigations**</span></span>

<span data-ttu-id="0b84f-482">如果連線需要保持開啟，則明確呼叫 `OpenConnection()` 可確保 EF Core 不會過早將其關閉：</span><span class="sxs-lookup"><span data-stu-id="0b84f-482">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();
        
        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

<a name="each"></a>

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="0b84f-483">每個屬性會使用獨立的記憶體內部整數索引鍵產生</span><span class="sxs-lookup"><span data-stu-id="0b84f-483">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="0b84f-484">追蹤問題 #6872</span><span class="sxs-lookup"><span data-stu-id="0b84f-484">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="0b84f-485">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-485">**Old behavior**</span></span>

<span data-ttu-id="0b84f-486">在 EF Core 3.0 以前，會針對所有記憶體內部整數索引鍵屬性使用一個共用值產生器。</span><span class="sxs-lookup"><span data-stu-id="0b84f-486">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="0b84f-487">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-487">**New behavior**</span></span>

<span data-ttu-id="0b84f-488">從 EF Core 3.0 開始，當使用記憶體內部資料庫時，每個整數索引鍵屬性都會取得自己的值產生器。</span><span class="sxs-lookup"><span data-stu-id="0b84f-488">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="0b84f-489">此外，如果已刪除資料庫，則會重設所有資料表的索引鍵產生。</span><span class="sxs-lookup"><span data-stu-id="0b84f-489">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="0b84f-490">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-490">**Why**</span></span>

<span data-ttu-id="0b84f-491">這項變更的目的是為了讓記憶體內部索引鍵產生與實際資料庫索引鍵產生更加一致，並改善在使用記憶體內部資料庫時隔離個別測試的能力。</span><span class="sxs-lookup"><span data-stu-id="0b84f-491">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="0b84f-492">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-492">**Mitigations**</span></span>

<span data-ttu-id="0b84f-493">這可能會中斷需要設定特定記憶體內部索引鍵值的應用程式。</span><span class="sxs-lookup"><span data-stu-id="0b84f-493">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="0b84f-494">請考慮改為不依賴特定索引鍵值，或更新以符合新行為。</span><span class="sxs-lookup"><span data-stu-id="0b84f-494">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="0b84f-495">預設會使用支援欄位</span><span class="sxs-lookup"><span data-stu-id="0b84f-495">Backing fields are used by default</span></span>

[<span data-ttu-id="0b84f-496">追蹤問題 #12430</span><span class="sxs-lookup"><span data-stu-id="0b84f-496">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="0b84f-497">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-497">**Old behavior**</span></span>

<span data-ttu-id="0b84f-498">在 3.0 以前，即使屬性的支援欄位已知，EF Core 預設仍會使用屬性 getter 和 setter 方法來讀取和寫入屬性值。</span><span class="sxs-lookup"><span data-stu-id="0b84f-498">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="0b84f-499">例外是查詢執行，其中如果支援欄位已知，則會直接設定。</span><span class="sxs-lookup"><span data-stu-id="0b84f-499">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="0b84f-500">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-500">**New behavior**</span></span>

<span data-ttu-id="0b84f-501">從 EF Core 3.0 開始，如果屬性的支援欄位已知，則 EF Core 會一律使用支援欄位來讀取和寫入該屬性。</span><span class="sxs-lookup"><span data-stu-id="0b84f-501">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="0b84f-502">如果應用程式需要將額外的行為編碼到 getter 或 setter 方法中，這可能會導致應用程式中斷。</span><span class="sxs-lookup"><span data-stu-id="0b84f-502">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="0b84f-503">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-503">**Why**</span></span>

<span data-ttu-id="0b84f-504">這項變更的目的是為了防止 EF Core 預設在執行涉及實體的資料庫作業時，錯誤地觸發商務邏輯。</span><span class="sxs-lookup"><span data-stu-id="0b84f-504">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="0b84f-505">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-505">**Mitigations**</span></span>

<span data-ttu-id="0b84f-506">透過在 `ModelBuilder` 上設定屬性存取模式可以還原 3.0 以前的行為。</span><span class="sxs-lookup"><span data-stu-id="0b84f-506">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="0b84f-507">例如:</span><span class="sxs-lookup"><span data-stu-id="0b84f-507">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="0b84f-508">找到多個相容的支援欄位時擲回</span><span class="sxs-lookup"><span data-stu-id="0b84f-508">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="0b84f-509">追蹤問題 #12523</span><span class="sxs-lookup"><span data-stu-id="0b84f-509">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="0b84f-510">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-510">**Old behavior**</span></span>

<span data-ttu-id="0b84f-511">在 EF Core 3.0 以前，如果有多個欄位符合尋找屬性支援欄位的規則，則會根據特定優先順序來選擇一個欄位。</span><span class="sxs-lookup"><span data-stu-id="0b84f-511">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="0b84f-512">這可能會導致在模稜兩可的情況下使用錯誤的欄位。</span><span class="sxs-lookup"><span data-stu-id="0b84f-512">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="0b84f-513">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-513">**New behavior**</span></span>

<span data-ttu-id="0b84f-514">從 EF Core 3.0 開始，如果有多個欄位符合相同的屬性，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0b84f-514">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="0b84f-515">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-515">**Why**</span></span>

<span data-ttu-id="0b84f-516">這項變更的目的是為了避免在只能有一個正確欄位的情況下，自動使用某個欄位而非另一個欄位。</span><span class="sxs-lookup"><span data-stu-id="0b84f-516">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="0b84f-517">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-517">**Mitigations**</span></span>

<span data-ttu-id="0b84f-518">若屬性的支援欄位模稜兩可，則必須明確指定要使用的欄位。</span><span class="sxs-lookup"><span data-stu-id="0b84f-518">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="0b84f-519">例如，使用 Fluent API：</span><span class="sxs-lookup"><span data-stu-id="0b84f-519">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="0b84f-520">僅限欄位的屬性名稱應與欄位名稱相符</span><span class="sxs-lookup"><span data-stu-id="0b84f-520">Field-only property names should match the field name</span></span>

<span data-ttu-id="0b84f-521">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-521">**Old behavior**</span></span>

<span data-ttu-id="0b84f-522">在 EF Core 3.0 之前，屬性可以由字串值指定，而且如果在 .NET 類型上找不到具有該名稱的屬性，則 EF Core 會嘗試使用慣例規則將它與欄位進行比對。</span><span class="sxs-lookup"><span data-stu-id="0b84f-522">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
```C#
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```
```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="0b84f-523">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-523">**New behavior**</span></span>

<span data-ttu-id="0b84f-524">從 EF Core 3.0 開始，僅限欄位的屬性必須與欄位名稱完全相符。</span><span class="sxs-lookup"><span data-stu-id="0b84f-524">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="0b84f-525">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-525">**Why**</span></span>

<span data-ttu-id="0b84f-526">此變更是為了避免對兩個名稱相似的屬性使用相同欄位，也使僅限欄位屬性之比對規則與對應至 CLR 屬性的屬性相同。</span><span class="sxs-lookup"><span data-stu-id="0b84f-526">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="0b84f-527">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-527">**Mitigations**</span></span>

<span data-ttu-id="0b84f-528">僅限欄位屬性必須命名為與其所對應欄位相同的名稱。</span><span class="sxs-lookup"><span data-stu-id="0b84f-528">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="0b84f-529">在3.0 以後的 EF Core 版本中，我們計畫重新啟用明確設定與屬性名稱不同的功能變數名稱（請參閱問題[#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)）：</span><span class="sxs-lookup"><span data-stu-id="0b84f-529">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="0b84f-530">AddDbContext/AddDbContextPool 再也不會呼叫 AddLogging 與 AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="0b84f-530">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="0b84f-531">追蹤問題 #14756</span><span class="sxs-lookup"><span data-stu-id="0b84f-531">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="0b84f-532">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-532">**Old behavior**</span></span>

<span data-ttu-id="0b84f-533">在 EF Core 3.0 之前，呼叫 `AddDbContext` 或 `AddDbContextPool` 也會透過呼叫 [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) 與 [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache) 來向 DI 註冊記錄與記憶體快取服務。</span><span class="sxs-lookup"><span data-stu-id="0b84f-533">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="0b84f-534">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-534">**New behavior**</span></span>

<span data-ttu-id="0b84f-535">從 EF Core 3.0 開始，`AddDbContext` 與 `AddDbContextPool` 再也不會向相依性插入 (DI) 註冊這些服務。</span><span class="sxs-lookup"><span data-stu-id="0b84f-535">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="0b84f-536">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-536">**Why**</span></span>

<span data-ttu-id="0b84f-537">EF Core 3.0 不會要求這些服務必須存在於應用程式的 DI 容器中。</span><span class="sxs-lookup"><span data-stu-id="0b84f-537">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="0b84f-538">不過，若 `ILoggerFactory` 已在應用程式的 DI 容器中註冊，則它仍會被 EF Core 使用。</span><span class="sxs-lookup"><span data-stu-id="0b84f-538">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="0b84f-539">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-539">**Mitigations**</span></span>

<span data-ttu-id="0b84f-540">若您的應用程式需要這些服務，請使用  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) 或 [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache) 明確地向 DI 容器註冊它們。</span><span class="sxs-lookup"><span data-stu-id="0b84f-540">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="0b84f-541">DbContext.Entry 現在會執行本機 DetectChanges</span><span class="sxs-lookup"><span data-stu-id="0b84f-541">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="0b84f-542">追蹤問題 #13552</span><span class="sxs-lookup"><span data-stu-id="0b84f-542">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="0b84f-543">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-543">**Old behavior**</span></span>

<span data-ttu-id="0b84f-544">在 EF Core 3.0 以前，呼叫 `DbContext.Entry` 會導致偵測所有追蹤實體的變更。</span><span class="sxs-lookup"><span data-stu-id="0b84f-544">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="0b84f-545">這可確保在 `EntityEntry` 中公開的狀態為最新狀態。</span><span class="sxs-lookup"><span data-stu-id="0b84f-545">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="0b84f-546">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-546">**New behavior**</span></span>

<span data-ttu-id="0b84f-547">從 EF Core 3.0 開始，呼叫 `DbContext.Entry` 現在只會嘗試在指定實體及其相關的任何追蹤主要實體中偵測變更。</span><span class="sxs-lookup"><span data-stu-id="0b84f-547">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="0b84f-548">這表示呼叫此方法可能還無法偵測到其他位置的變更，因此可能會影響應用程式狀態。</span><span class="sxs-lookup"><span data-stu-id="0b84f-548">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="0b84f-549">請注意，如果 `ChangeTracker.AutoDetectChangesEnabled` 設定為 `false`，甚至是此本機變更偵測都會停用。</span><span class="sxs-lookup"><span data-stu-id="0b84f-549">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="0b84f-550">其他導致變更偵測的方法 (例如 `ChangeTracker.Entries` 和 `SaveChanges`) 仍會對所有追蹤實體進行完整的 `DetectChanges`。</span><span class="sxs-lookup"><span data-stu-id="0b84f-550">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="0b84f-551">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-551">**Why**</span></span>

<span data-ttu-id="0b84f-552">這項變更的目的是為了改善使用 `context.Entry` 的預設效能。</span><span class="sxs-lookup"><span data-stu-id="0b84f-552">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="0b84f-553">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-553">**Mitigations**</span></span>

<span data-ttu-id="0b84f-554">在呼叫 `Entry` 之前明確呼叫 `ChgangeTracker.DetectChanges()` 可確保 3.0 以前的行為。</span><span class="sxs-lookup"><span data-stu-id="0b84f-554">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="0b84f-555">字串和位元組陣列索引鍵預設不是由用戶端產生</span><span class="sxs-lookup"><span data-stu-id="0b84f-555">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="0b84f-556">追蹤問題 #14617</span><span class="sxs-lookup"><span data-stu-id="0b84f-556">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="0b84f-557">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-557">**Old behavior**</span></span>

<span data-ttu-id="0b84f-558">在 EF Core 3.0 以前，可以使用 `string` 和 `byte[]` 索引鍵屬性，而不需要明確設定非 Null 值。</span><span class="sxs-lookup"><span data-stu-id="0b84f-558">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="0b84f-559">在此情況下，會在用戶端以 GUID 形式產生索引鍵值，再序列化為 `byte[]` 的位元組。</span><span class="sxs-lookup"><span data-stu-id="0b84f-559">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="0b84f-560">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-560">**New behavior**</span></span>

<span data-ttu-id="0b84f-561">從 EF Core 3.0 開始，系統會擲回例外狀況，指出尚未設定任何索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="0b84f-561">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="0b84f-562">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-562">**Why**</span></span>

<span data-ttu-id="0b84f-563">這項變更是因為用戶端產生的 `string`/`byte[]` 值通常不太有用，而且預設行為使它很難以一般方式來推論產生的索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="0b84f-563">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="0b84f-564">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-564">**Mitigations**</span></span>

<span data-ttu-id="0b84f-565">藉由明確指定索引鍵屬性應該在未設定其他非 Null 值時使用產生的值，即可取得 3.0 以前的行為。</span><span class="sxs-lookup"><span data-stu-id="0b84f-565">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="0b84f-566">例如，使用 Fluent API：</span><span class="sxs-lookup"><span data-stu-id="0b84f-566">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="0b84f-567">或者，使用資料註解：</span><span class="sxs-lookup"><span data-stu-id="0b84f-567">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="0b84f-568">ILoggerFactory 現在是限定範圍的服務</span><span class="sxs-lookup"><span data-stu-id="0b84f-568">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="0b84f-569">追蹤問題 #14698</span><span class="sxs-lookup"><span data-stu-id="0b84f-569">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="0b84f-570">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-570">**Old behavior**</span></span>

<span data-ttu-id="0b84f-571">在 EF Core 3.0 以前，`ILoggerFactory` 會註冊為單一服務。</span><span class="sxs-lookup"><span data-stu-id="0b84f-571">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="0b84f-572">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-572">**New behavior**</span></span>

<span data-ttu-id="0b84f-573">從 EF Core 3.0 開始，`ILoggerFactory` 現在會註冊為限定範圍。</span><span class="sxs-lookup"><span data-stu-id="0b84f-573">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="0b84f-574">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-574">**Why**</span></span>

<span data-ttu-id="0b84f-575">這項變更的目的是為了允許記錄器與 `DbContext` 執行個體產生關聯，這可啟用其他功能，並避免某些異常行為案例，例如內部服務提供者遽增。</span><span class="sxs-lookup"><span data-stu-id="0b84f-575">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="0b84f-576">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-576">**Mitigations**</span></span>

<span data-ttu-id="0b84f-577">這項變更不應影響應用程式程式碼，除非在 EF Core 內部服務提供者上使用自訂服務註冊該程式碼。</span><span class="sxs-lookup"><span data-stu-id="0b84f-577">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="0b84f-578">但這並不常見。</span><span class="sxs-lookup"><span data-stu-id="0b84f-578">This isn't common.</span></span>
<span data-ttu-id="0b84f-579">在這些情況下，大部分的項目仍會運作，但相依於 `ILoggerFactory` 的任何單一服務需要變更，才能以不同方式取得 `ILoggerFactory`。</span><span class="sxs-lookup"><span data-stu-id="0b84f-579">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="0b84f-580">如果您遇到上述情況，請在 [EF Core GitHub 問題追蹤器](https://github.com/aspnet/EntityFrameworkCore/issues)上提出問題，讓我們知道您使用 `ILoggerFactory` 的方式，以便進一步了解未來如何才不會再次中斷。</span><span class="sxs-lookup"><span data-stu-id="0b84f-580">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="0b84f-581">消極式載入 Proxy 停止假設導覽屬性已完全載入</span><span class="sxs-lookup"><span data-stu-id="0b84f-581">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="0b84f-582">追蹤問題 #12780</span><span class="sxs-lookup"><span data-stu-id="0b84f-582">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="0b84f-583">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-583">**Old behavior**</span></span>

<span data-ttu-id="0b84f-584">在 EF Core 3.0 以前，一旦處置 `DbContext` 之後，就無從得知實體上取自該內容的指定導覽屬性是否已完全載入。</span><span class="sxs-lookup"><span data-stu-id="0b84f-584">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="0b84f-585">Proxy 會改為假設如有非 Null 值，會載入參考導覽；如果不是空的，則會載入集合導覽。</span><span class="sxs-lookup"><span data-stu-id="0b84f-585">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="0b84f-586">在這些情況下，嘗試消極式載入不會執行任何作業。</span><span class="sxs-lookup"><span data-stu-id="0b84f-586">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="0b84f-587">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-587">**New behavior**</span></span>

<span data-ttu-id="0b84f-588">從 EF Core 3.0 開始，Proxy 會追蹤是否載入導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="0b84f-588">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="0b84f-589">這表示嘗試存取在處置內容之後載入的導覽屬性一律不會執行任何作業，即使已載入的導覽是空的或 Null 也一樣。</span><span class="sxs-lookup"><span data-stu-id="0b84f-589">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="0b84f-590">相反地，如果在處置內容之後嘗試存取未載入的導覽屬性，則會擲回例外狀況，即使導覽屬性不是空集合也一樣。</span><span class="sxs-lookup"><span data-stu-id="0b84f-590">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="0b84f-591">如果發生這種情況，則表示應用程式程式碼嘗試在無效的時間使用消極式載入，應用程式應該變更為不要這麼做。</span><span class="sxs-lookup"><span data-stu-id="0b84f-591">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="0b84f-592">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-592">**Why**</span></span>

<span data-ttu-id="0b84f-593">這項變更的目的是為了在已處置的 `DbContext` 執行個體上嘗試消極式載入時，使行為一致且正確。</span><span class="sxs-lookup"><span data-stu-id="0b84f-593">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="0b84f-594">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-594">**Mitigations**</span></span>

<span data-ttu-id="0b84f-595">將應用程式程式碼更新為不要嘗試對已處置的內容進行消極式載入，或將此設定為不執行任何作業，如例外狀況訊息中所述。</span><span class="sxs-lookup"><span data-stu-id="0b84f-595">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="0b84f-596">過度建立內部服務提供者現在預設是錯誤</span><span class="sxs-lookup"><span data-stu-id="0b84f-596">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="0b84f-597">追蹤問題 #10236</span><span class="sxs-lookup"><span data-stu-id="0b84f-597">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="0b84f-598">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-598">**Old behavior**</span></span>

<span data-ttu-id="0b84f-599">在 EF Core 3.0 以前，當應用程式建立異常數目的內部服務提供者時，會記錄一則警告。</span><span class="sxs-lookup"><span data-stu-id="0b84f-599">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="0b84f-600">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-600">**New behavior**</span></span>

<span data-ttu-id="0b84f-601">從 EF Core 3.0 開始，此警告現在會視為錯誤，並會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0b84f-601">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="0b84f-602">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-602">**Why**</span></span>

<span data-ttu-id="0b84f-603">這項變更的目的是為了透過更明確公開此異常案例，藉以開發更完善的應用程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="0b84f-603">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="0b84f-604">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-604">**Mitigations**</span></span>

<span data-ttu-id="0b84f-605">遇到此錯誤時的最適當動作是了解根本原因，並停止建立這麼多的內部服務提供者。</span><span class="sxs-lookup"><span data-stu-id="0b84f-605">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="0b84f-606">不過，透過設定 `DbContextOptionsBuilder` 可以將錯誤轉換回警告。</span><span class="sxs-lookup"><span data-stu-id="0b84f-606">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="0b84f-607">例如:</span><span class="sxs-lookup"><span data-stu-id="0b84f-607">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="0b84f-608">使用單一字串呼叫之 HasOne/HasMany 的新行為</span><span class="sxs-lookup"><span data-stu-id="0b84f-608">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="0b84f-609">追蹤問題 #9171</span><span class="sxs-lookup"><span data-stu-id="0b84f-609">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="0b84f-610">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-610">**Old behavior**</span></span>

<span data-ttu-id="0b84f-611">在 EF Core 3.0 之前，使用單一字串呼叫 `HasOne` 或 `HasMany` 的程式碼會以令人困惑的方式解譯。</span><span class="sxs-lookup"><span data-stu-id="0b84f-611">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="0b84f-612">例如:</span><span class="sxs-lookup"><span data-stu-id="0b84f-612">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="0b84f-613">程式碼看起來像是它使用 `Entrance` 瀏覽屬性將 `Samurai` 與一些其他實體類型相關，這可能是私用屬性。</span><span class="sxs-lookup"><span data-stu-id="0b84f-613">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="0b84f-614">在現實中，此程式碼會在不使用瀏覽屬性的情況下嘗試建立與一些實體 (稱為 `Entrance`) 的關係。</span><span class="sxs-lookup"><span data-stu-id="0b84f-614">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="0b84f-615">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-615">**New behavior**</span></span>

<span data-ttu-id="0b84f-616">從 EF Core 3.0 開始，上述程式碼現在會執行像以前一樣的動作。</span><span class="sxs-lookup"><span data-stu-id="0b84f-616">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="0b84f-617">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-617">**Why**</span></span>

<span data-ttu-id="0b84f-618">舊行為令人非常困惑，特別是當讀取設定程式碼與尋找錯誤時。</span><span class="sxs-lookup"><span data-stu-id="0b84f-618">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="0b84f-619">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-619">**Mitigations**</span></span>

<span data-ttu-id="0b84f-620">這只會造成已明確針對類型名稱使用字串設定關係，而未明確指定瀏覽屬性的應用程式中斷。</span><span class="sxs-lookup"><span data-stu-id="0b84f-620">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="0b84f-621">這不是常見情況。</span><span class="sxs-lookup"><span data-stu-id="0b84f-621">This is not common.</span></span>
<span data-ttu-id="0b84f-622">先前的行為可透過明確地傳遞瀏覽屬性名稱的 `null` 來取得。</span><span class="sxs-lookup"><span data-stu-id="0b84f-622">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="0b84f-623">例如:</span><span class="sxs-lookup"><span data-stu-id="0b84f-623">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="0b84f-624">數個非同步方法的傳回類型已從 Task 變更為 ValueTask</span><span class="sxs-lookup"><span data-stu-id="0b84f-624">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="0b84f-625">追蹤問題 #15184</span><span class="sxs-lookup"><span data-stu-id="0b84f-625">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="0b84f-626">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-626">**Old behavior**</span></span>

<span data-ttu-id="0b84f-627">下列非同步方法先前傳回了 `Task<T>`：</span><span class="sxs-lookup"><span data-stu-id="0b84f-627">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="0b84f-628">`ValueGenerator.NextValueAsync()` (和衍生類別)</span><span class="sxs-lookup"><span data-stu-id="0b84f-628">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="0b84f-629">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-629">**New behavior**</span></span>

<span data-ttu-id="0b84f-630">上述方法現在會透過相同的 `T` 傳回 `ValueTask<T>`，如同以前一樣。</span><span class="sxs-lookup"><span data-stu-id="0b84f-630">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="0b84f-631">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-631">**Why**</span></span>

<span data-ttu-id="0b84f-632">這項變更可降低叫用這些方法時產生的堆積配置數目，可改善一般效能。</span><span class="sxs-lookup"><span data-stu-id="0b84f-632">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="0b84f-633">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-633">**Mitigations**</span></span>

<span data-ttu-id="0b84f-634">僅等待上述 API 的應用程式只需要重新編譯，而不需要變更來源。</span><span class="sxs-lookup"><span data-stu-id="0b84f-634">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="0b84f-635">更複雜的使用方式 (例如將傳回的 `Task` 傳遞給 `Task.WhenAny()`) 通常需要藉由呼叫 `AsTask()` 將傳回的 `ValueTask<T>` 轉換為 `Task<T>`。</span><span class="sxs-lookup"><span data-stu-id="0b84f-635">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="0b84f-636">請注意，這會抵消這項變更所帶來的配置減少。</span><span class="sxs-lookup"><span data-stu-id="0b84f-636">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="0b84f-637">Relational:TypeMapping 註解現僅為 TypeMapping</span><span class="sxs-lookup"><span data-stu-id="0b84f-637">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="0b84f-638">追蹤問題 #9913</span><span class="sxs-lookup"><span data-stu-id="0b84f-638">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="0b84f-639">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-639">**Old behavior**</span></span>

<span data-ttu-id="0b84f-640">類型對應註解的註解名稱之前是 "Relational:TypeMapping"。</span><span class="sxs-lookup"><span data-stu-id="0b84f-640">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="0b84f-641">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-641">**New behavior**</span></span>

<span data-ttu-id="0b84f-642">類型對應註解的註解名稱現在是 "TypeMapping"。</span><span class="sxs-lookup"><span data-stu-id="0b84f-642">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="0b84f-643">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-643">**Why**</span></span>

<span data-ttu-id="0b84f-644">類型對應現在不只用於關聯式資料庫提供者。</span><span class="sxs-lookup"><span data-stu-id="0b84f-644">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="0b84f-645">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-645">**Mitigations**</span></span>

<span data-ttu-id="0b84f-646">這只會中斷直接將類型對應當做註解存取的應用程式，但這並不常見。</span><span class="sxs-lookup"><span data-stu-id="0b84f-646">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="0b84f-647">最適當的修正動作是使用 API 介面存取類型對應，而不是直接使用註解。</span><span class="sxs-lookup"><span data-stu-id="0b84f-647">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="0b84f-648">衍生類型上的 ToTable 會擲回例外狀況</span><span class="sxs-lookup"><span data-stu-id="0b84f-648">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="0b84f-649">追蹤問題 #11811</span><span class="sxs-lookup"><span data-stu-id="0b84f-649">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="0b84f-650">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-650">**Old behavior**</span></span>

<span data-ttu-id="0b84f-651">在 EF Core 3.0 以前，會忽略衍生類型上呼叫的 `ToTable()`，因為唯一的繼承對應策略是對此案例無效的 TPH。</span><span class="sxs-lookup"><span data-stu-id="0b84f-651">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="0b84f-652">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-652">**New behavior**</span></span>

<span data-ttu-id="0b84f-653">從 EF Core 3.0 開始，以及在更新版本中準備新增 TPT 和 TPC 支援時，在衍生類型上呼叫的 `ToTable()` 現在會擲回例外狀況，以避免未來發生非預期的對應變更。</span><span class="sxs-lookup"><span data-stu-id="0b84f-653">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="0b84f-654">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-654">**Why**</span></span>

<span data-ttu-id="0b84f-655">目前無法將衍生類型對應至不同的資料表。</span><span class="sxs-lookup"><span data-stu-id="0b84f-655">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="0b84f-656">這項變更可避免未來有效執行時的中斷情況。</span><span class="sxs-lookup"><span data-stu-id="0b84f-656">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="0b84f-657">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-657">**Mitigations**</span></span>

<span data-ttu-id="0b84f-658">避免嘗試將衍生類型對應至其他資料表。</span><span class="sxs-lookup"><span data-stu-id="0b84f-658">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="0b84f-659">ForSqlServerHasIndex 已取代為 HasIndex</span><span class="sxs-lookup"><span data-stu-id="0b84f-659">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="0b84f-660">追蹤問題 #12366</span><span class="sxs-lookup"><span data-stu-id="0b84f-660">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="0b84f-661">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-661">**Old behavior**</span></span>

<span data-ttu-id="0b84f-662">在 EF Core 3.0 以前，`ForSqlServerHasIndex().ForSqlServerInclude()` 可讓您設定搭配 `INCLUDE` 使用的資料行。</span><span class="sxs-lookup"><span data-stu-id="0b84f-662">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="0b84f-663">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-663">**New behavior**</span></span>

<span data-ttu-id="0b84f-664">從 EF Core 3.0 開始，關聯式層級現在支援對索引使用 `Include`。</span><span class="sxs-lookup"><span data-stu-id="0b84f-664">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="0b84f-665">使用 `HasIndex().ForSqlServerInclude()`。</span><span class="sxs-lookup"><span data-stu-id="0b84f-665">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="0b84f-666">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-666">**Why**</span></span>

<span data-ttu-id="0b84f-667">這項變更的目的是為了能夠使用 `Include` 將所有資料庫提供者的索引 API 合併到一個位置。</span><span class="sxs-lookup"><span data-stu-id="0b84f-667">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="0b84f-668">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-668">**Mitigations**</span></span>

<span data-ttu-id="0b84f-669">使用新的 API，如上所示。</span><span class="sxs-lookup"><span data-stu-id="0b84f-669">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="0b84f-670">中繼資料 API 變更</span><span class="sxs-lookup"><span data-stu-id="0b84f-670">Metadata API changes</span></span>

[<span data-ttu-id="0b84f-671">追蹤問題 #214</span><span class="sxs-lookup"><span data-stu-id="0b84f-671">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="0b84f-672">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-672">**New behavior**</span></span>

<span data-ttu-id="0b84f-673">下列屬性已轉換為擴充方法：</span><span class="sxs-lookup"><span data-stu-id="0b84f-673">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="0b84f-674">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-674">**Why**</span></span>

<span data-ttu-id="0b84f-675">這項變更可簡化上述介面的實作。</span><span class="sxs-lookup"><span data-stu-id="0b84f-675">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="0b84f-676">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-676">**Mitigations**</span></span>

<span data-ttu-id="0b84f-677">使用新的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="0b84f-677">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="0b84f-678">提供者特定的中繼資料 API 變更</span><span class="sxs-lookup"><span data-stu-id="0b84f-678">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="0b84f-679">追蹤問題 #214</span><span class="sxs-lookup"><span data-stu-id="0b84f-679">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="0b84f-680">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-680">**New behavior**</span></span>

<span data-ttu-id="0b84f-681">提供者特定的擴充方法會壓平合併：</span><span class="sxs-lookup"><span data-stu-id="0b84f-681">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="0b84f-682">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-682">**Why**</span></span>

<span data-ttu-id="0b84f-683">此變更可簡化上述延伸方法的實作。</span><span class="sxs-lookup"><span data-stu-id="0b84f-683">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="0b84f-684">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-684">**Mitigations**</span></span>

<span data-ttu-id="0b84f-685">使用新的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="0b84f-685">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="0b84f-686">EF Core 不會再傳送 SQLite FK 強制的 pragma</span><span class="sxs-lookup"><span data-stu-id="0b84f-686">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="0b84f-687">追蹤問題 #12151</span><span class="sxs-lookup"><span data-stu-id="0b84f-687">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="0b84f-688">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-688">**Old behavior**</span></span>

<span data-ttu-id="0b84f-689">在 EF Core 3.0 以前，當開啟 SQLite 連線時，EF Core 會傳送 `PRAGMA foreign_keys = 1`。</span><span class="sxs-lookup"><span data-stu-id="0b84f-689">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="0b84f-690">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-690">**New behavior**</span></span>

<span data-ttu-id="0b84f-691">從 EF Core 3.0 開始，當開啟 SQLite 連線時，EF Core 不會再傳送 `PRAGMA foreign_keys = 1`。</span><span class="sxs-lookup"><span data-stu-id="0b84f-691">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="0b84f-692">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-692">**Why**</span></span>

<span data-ttu-id="0b84f-693">這項變更是因為 EF Core 預設會使用 `SQLitePCLRaw.bundle_e_sqlite3`，這也表示預設會開啟 FK 強制，而不需要在每次開啟連線時明確啟用。</span><span class="sxs-lookup"><span data-stu-id="0b84f-693">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="0b84f-694">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-694">**Mitigations**</span></span>

<span data-ttu-id="0b84f-695">根據預設，會在預設用於 EF Core 的 SQLitePCLRaw.bundle_e_sqlite3 中啟用外部索引鍵。</span><span class="sxs-lookup"><span data-stu-id="0b84f-695">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="0b84f-696">在其他情況下，則可以藉由在您的連接字串中指定 `Foreign Keys=True` 來啟用外部索引鍵。</span><span class="sxs-lookup"><span data-stu-id="0b84f-696">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="0b84f-697">Microsoft.EntityFrameworkCore.Sqlite 現在相依於 SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="0b84f-697">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="0b84f-698">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-698">**Old behavior**</span></span>

<span data-ttu-id="0b84f-699">在 EF Core 3.0 以前，EF Core 會使用 `SQLitePCLRaw.bundle_green`。</span><span class="sxs-lookup"><span data-stu-id="0b84f-699">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="0b84f-700">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-700">**New behavior**</span></span>

<span data-ttu-id="0b84f-701">從 EF Core 3.0 開始，EF Core 會使用 `SQLitePCLRaw.bundle_e_sqlite3`。</span><span class="sxs-lookup"><span data-stu-id="0b84f-701">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="0b84f-702">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-702">**Why**</span></span>

<span data-ttu-id="0b84f-703">這項變更的目的是為了讓用於 iOS 的 SQLite 版本與其他平台一致。</span><span class="sxs-lookup"><span data-stu-id="0b84f-703">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="0b84f-704">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-704">**Mitigations**</span></span>

<span data-ttu-id="0b84f-705">若要在 iOS 上使用原生 SQLite 版本，請設定 `Microsoft.Data.Sqlite` 使用不同的 `SQLitePCLRaw` 套件組合。</span><span class="sxs-lookup"><span data-stu-id="0b84f-705">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="0b84f-706">GUID 值現在於 SQLite 上的儲存形式為 TEXT</span><span class="sxs-lookup"><span data-stu-id="0b84f-706">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="0b84f-707">追蹤問題 #15078</span><span class="sxs-lookup"><span data-stu-id="0b84f-707">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="0b84f-708">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-708">**Old behavior**</span></span>

<span data-ttu-id="0b84f-709">GUID 值先前在 SQLite 上的儲存形式為 BLOB 值。</span><span class="sxs-lookup"><span data-stu-id="0b84f-709">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="0b84f-710">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-710">**New behavior**</span></span>

<span data-ttu-id="0b84f-711">GUID 值現在會儲存為 TEXT。</span><span class="sxs-lookup"><span data-stu-id="0b84f-711">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="0b84f-712">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-712">**Why**</span></span>

<span data-ttu-id="0b84f-713">GUID 的二進位格式未標準化。</span><span class="sxs-lookup"><span data-stu-id="0b84f-713">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="0b84f-714">以 TEXT 的形式儲存值會提高資料庫與其他技術的相容性。</span><span class="sxs-lookup"><span data-stu-id="0b84f-714">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="0b84f-715">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-715">**Mitigations**</span></span>

<span data-ttu-id="0b84f-716">您可以參考以下方式執行 SQL，來將現有的資料庫移轉至新的格式。</span><span class="sxs-lookup"><span data-stu-id="0b84f-716">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

<span data-ttu-id="0b84f-717">在 EF Core 中，您也可以在這些屬性上設定值轉換器來繼續使用原本的行為。</span><span class="sxs-lookup"><span data-stu-id="0b84f-717">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="0b84f-718">Microsoft.Data.Sqlite 依然可以同時從 BLOB 及 TEXT 資料行讀取 GUID 值；但因為參數和常數的預設格式已變更，所以您可能需要對多數涉及 GUID 的案例採取動作。</span><span class="sxs-lookup"><span data-stu-id="0b84f-718">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="0b84f-719">Char 值現在於 SQLite 上會儲存為文字</span><span class="sxs-lookup"><span data-stu-id="0b84f-719">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="0b84f-720">追蹤問題 #15020</span><span class="sxs-lookup"><span data-stu-id="0b84f-720">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="0b84f-721">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-721">**Old behavior**</span></span>

<span data-ttu-id="0b84f-722">Char 值原先在 SQLite 上儲存為整數值。</span><span class="sxs-lookup"><span data-stu-id="0b84f-722">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="0b84f-723">舉例來說，char 值 *A* 原先會儲存為整數值 65。</span><span class="sxs-lookup"><span data-stu-id="0b84f-723">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="0b84f-724">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-724">**New behavior**</span></span>

<span data-ttu-id="0b84f-725">Char 值現在會儲存為 TEXT。</span><span class="sxs-lookup"><span data-stu-id="0b84f-725">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="0b84f-726">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-726">**Why**</span></span>

<span data-ttu-id="0b84f-727">將值儲存為 TEXT 不但更加自然，也使資料庫與其他技術的相容性更高。</span><span class="sxs-lookup"><span data-stu-id="0b84f-727">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="0b84f-728">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-728">**Mitigations**</span></span>

<span data-ttu-id="0b84f-729">您可以參考以下方式執行 SQL，來將現有的資料庫移轉至新的格式。</span><span class="sxs-lookup"><span data-stu-id="0b84f-729">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="0b84f-730">在 EF Core 中，您也可以在這些屬性上設定值轉換器來繼續使用原本的行為。</span><span class="sxs-lookup"><span data-stu-id="0b84f-730">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="0b84f-731">Microsoft.Data.Sqlite 也保留了讀取 INTEGER 和 TEXT 欄位字元值的功能，所以部分案例可能不需要任何動作。</span><span class="sxs-lookup"><span data-stu-id="0b84f-731">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="0b84f-732">移轉識別碼現在會使用不因文化特性而異的行事曆產生</span><span class="sxs-lookup"><span data-stu-id="0b84f-732">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="0b84f-733">追蹤問題 #12978</span><span class="sxs-lookup"><span data-stu-id="0b84f-733">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="0b84f-734">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-734">**Old behavior**</span></span>

<span data-ttu-id="0b84f-735">移轉識別碼原先會使用目前文化特性 (Culture) 的行事曆產生。</span><span class="sxs-lookup"><span data-stu-id="0b84f-735">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="0b84f-736">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-736">**New behavior**</span></span>

<span data-ttu-id="0b84f-737">移轉識別碼現在一律會使用不因文化特性而異的行事曆 (西曆) 產生。</span><span class="sxs-lookup"><span data-stu-id="0b84f-737">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="0b84f-738">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-738">**Why**</span></span>

<span data-ttu-id="0b84f-739">更新資料庫或解決合併衝突時，移轉的順序相當重要。</span><span class="sxs-lookup"><span data-stu-id="0b84f-739">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="0b84f-740">使用無差異的行事曆可避免順序問題，使小組成員系統行事曆不同的問題不會發生。</span><span class="sxs-lookup"><span data-stu-id="0b84f-740">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="0b84f-741">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-741">**Mitigations**</span></span>

<span data-ttu-id="0b84f-742">此變更會影響年份大於西曆行事曆的非西曆行事曆使用者 (例如泰國佛曆)。</span><span class="sxs-lookup"><span data-stu-id="0b84f-742">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="0b84f-743">現有的移轉識別碼必須更新，以使新的移轉會在現有的移轉之後排序。</span><span class="sxs-lookup"><span data-stu-id="0b84f-743">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="0b84f-744">您可在移轉設計工具檔案的移轉屬性中找到移轉識別碼。</span><span class="sxs-lookup"><span data-stu-id="0b84f-744">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="0b84f-745">移轉歷程記錄資料表也必須更新。</span><span class="sxs-lookup"><span data-stu-id="0b84f-745">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="0b84f-746">已移除 UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="0b84f-746">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="0b84f-747">追蹤問題 #16400</span><span class="sxs-lookup"><span data-stu-id="0b84f-747">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="0b84f-748">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-748">**Old behavior**</span></span>

<span data-ttu-id="0b84f-749">在 EF Core 3.0 之前，`UseRowNumberForPaging` 可用來問與 SQL Server 2008 相容的分頁產生 SQL。</span><span class="sxs-lookup"><span data-stu-id="0b84f-749">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="0b84f-750">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-750">**New behavior**</span></span>

<span data-ttu-id="0b84f-751">從 EF Core 3.0 開始，EF 將只會針對與新版 SQL Server 相容的分頁產生 SQL。</span><span class="sxs-lookup"><span data-stu-id="0b84f-751">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="0b84f-752">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-752">**Why**</span></span>

<span data-ttu-id="0b84f-753">我們正在進行此變更，因為 [SQL Server 2008 不再是支援的產品](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) \(英文\) 且更新此功能以搭配 EF Core 3.0 中的查詢變更使用是一個大工程。</span><span class="sxs-lookup"><span data-stu-id="0b84f-753">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="0b84f-754">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-754">**Mitigations**</span></span>

<span data-ttu-id="0b84f-755">我們建議更新為新版 SQL Server，或使用較高的相容性層級，以支援產生的 SQL。</span><span class="sxs-lookup"><span data-stu-id="0b84f-755">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="0b84f-756">儘管如此，若您無法這樣做，請[在追蹤問題下註解](https://github.com/aspnet/EntityFrameworkCore/issues/16400)並提供詳細資料。</span><span class="sxs-lookup"><span data-stu-id="0b84f-756">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="0b84f-757">我們可能會根據意見反應重新審視此決定。</span><span class="sxs-lookup"><span data-stu-id="0b84f-757">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="0b84f-758">已從 IDbContextOptionsExtension 移除延伸模組資訊/中繼資料</span><span class="sxs-lookup"><span data-stu-id="0b84f-758">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="0b84f-759">追蹤問題 #16119</span><span class="sxs-lookup"><span data-stu-id="0b84f-759">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="0b84f-760">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-760">**Old behavior**</span></span>

<span data-ttu-id="0b84f-761">用於提供有關延伸模組織中繼資料的 `IDbContextOptionsExtension` 包含方法。</span><span class="sxs-lookup"><span data-stu-id="0b84f-761">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="0b84f-762">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-762">**New behavior**</span></span>

<span data-ttu-id="0b84f-763">這些方法已移動到新的 `DbContextOptionsExtensionInfo` 抽象基底類別，這是從新的 `IDbContextOptionsExtension.Info` 屬性傳回的。</span><span class="sxs-lookup"><span data-stu-id="0b84f-763">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="0b84f-764">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-764">**Why**</span></span>

<span data-ttu-id="0b84f-765">在從 2.0 升級到 3.0 的程序中，我們必須新增或變更這些方法數次。</span><span class="sxs-lookup"><span data-stu-id="0b84f-765">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="0b84f-766">將它們分成新的抽象基底類別可讓我們更輕鬆地在不變更現有延伸模組的情況下進行此類變更。</span><span class="sxs-lookup"><span data-stu-id="0b84f-766">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="0b84f-767">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-767">**Mitigations**</span></span>

<span data-ttu-id="0b84f-768">更新延伸模組以遵循新模式。</span><span class="sxs-lookup"><span data-stu-id="0b84f-768">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="0b84f-769">您可以在 EF Core 原始程式碼中各種不同延伸模組之 `IDbContextOptionsExtension` 的許多實作中找到範例。</span><span class="sxs-lookup"><span data-stu-id="0b84f-769">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="0b84f-770">已重新命名 LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="0b84f-770">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="0b84f-771">追蹤問題 #10985</span><span class="sxs-lookup"><span data-stu-id="0b84f-771">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="0b84f-772">**變更**</span><span class="sxs-lookup"><span data-stu-id="0b84f-772">**Change**</span></span>

<span data-ttu-id="0b84f-773">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` 已經重新命名為 `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`。</span><span class="sxs-lookup"><span data-stu-id="0b84f-773">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="0b84f-774">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-774">**Why**</span></span>

<span data-ttu-id="0b84f-775">使這個警告事件的命名與其他所有警告事件一致。</span><span class="sxs-lookup"><span data-stu-id="0b84f-775">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="0b84f-776">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-776">**Mitigations**</span></span>

<span data-ttu-id="0b84f-777">使用新的名稱。</span><span class="sxs-lookup"><span data-stu-id="0b84f-777">Use the new name.</span></span> <span data-ttu-id="0b84f-778">(注意，事件識別碼未變更。)</span><span class="sxs-lookup"><span data-stu-id="0b84f-778">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="0b84f-779">讓 API 的外部索引鍵限制式名稱更清楚</span><span class="sxs-lookup"><span data-stu-id="0b84f-779">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="0b84f-780">追蹤問題 #10730</span><span class="sxs-lookup"><span data-stu-id="0b84f-780">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="0b84f-781">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-781">**Old behavior**</span></span>

<span data-ttu-id="0b84f-782">在 EF Core 3.0 前，外部索引鍵限制式名稱僅為 "name"。</span><span class="sxs-lookup"><span data-stu-id="0b84f-782">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="0b84f-783">例如:</span><span class="sxs-lookup"><span data-stu-id="0b84f-783">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="0b84f-784">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-784">**New behavior**</span></span>

<span data-ttu-id="0b84f-785">從 EF Core 3.0 開始，外部索引鍵限制式名稱現為 "constraint name"。</span><span class="sxs-lookup"><span data-stu-id="0b84f-785">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="0b84f-786">例如:</span><span class="sxs-lookup"><span data-stu-id="0b84f-786">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="0b84f-787">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-787">**Why**</span></span>

<span data-ttu-id="0b84f-788">此變更可讓此領域中的命名一致，同時清楚指出這是外部索引鍵限制式的名稱，而非定義外部索引鍵的資料行或屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="0b84f-788">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="0b84f-789">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-789">**Mitigations**</span></span>

<span data-ttu-id="0b84f-790">使用新的名稱。</span><span class="sxs-lookup"><span data-stu-id="0b84f-790">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="0b84f-791">IRelationalDatabaseCreator.HasTables/HasTablesAsync 已設定為公用</span><span class="sxs-lookup"><span data-stu-id="0b84f-791">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="0b84f-792">追蹤問題 #15997</span><span class="sxs-lookup"><span data-stu-id="0b84f-792">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="0b84f-793">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-793">**Old behavior**</span></span>

<span data-ttu-id="0b84f-794">在 EF Core 3.0 之前，這些方法已受保護。</span><span class="sxs-lookup"><span data-stu-id="0b84f-794">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="0b84f-795">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-795">**New behavior**</span></span>

<span data-ttu-id="0b84f-796">從 EF Core 3.0 開始，這些方法為公用。</span><span class="sxs-lookup"><span data-stu-id="0b84f-796">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="0b84f-797">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-797">**Why**</span></span>

<span data-ttu-id="0b84f-798">這些方法是由 EF 用來判斷資料庫是否已建立但為空資料庫。</span><span class="sxs-lookup"><span data-stu-id="0b84f-798">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="0b84f-799">當判斷是否要套用移轉時，這在 EF 外部也很實用。</span><span class="sxs-lookup"><span data-stu-id="0b84f-799">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="0b84f-800">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-800">**Mitigations**</span></span>

<span data-ttu-id="0b84f-801">變更任何覆寫的可存取性。</span><span class="sxs-lookup"><span data-stu-id="0b84f-801">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="0b84f-802">Microsoft.EntityFrameworkCore.Design 現在是 DevelopmentDependency 套件</span><span class="sxs-lookup"><span data-stu-id="0b84f-802">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="0b84f-803">追蹤問題 #11506</span><span class="sxs-lookup"><span data-stu-id="0b84f-803">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="0b84f-804">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-804">**Old behavior**</span></span>

<span data-ttu-id="0b84f-805">在 EF Core 3.0 之前，Microsoft.EntityFrameworkCore.Design 是標準 NuGet 套件，其組件可由相依於它的的專案參考。</span><span class="sxs-lookup"><span data-stu-id="0b84f-805">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="0b84f-806">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-806">**New behavior**</span></span>

<span data-ttu-id="0b84f-807">從 EF Core 3.0 開始，它是 DevelopmentDependency 套件。</span><span class="sxs-lookup"><span data-stu-id="0b84f-807">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="0b84f-808">這表示相依性將不會以可轉移方式流動到其他專案，而且您預設再也無法參考其組件。</span><span class="sxs-lookup"><span data-stu-id="0b84f-808">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="0b84f-809">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-809">**Why**</span></span>

<span data-ttu-id="0b84f-810">此套件旨在用於設計階段。</span><span class="sxs-lookup"><span data-stu-id="0b84f-810">This package is only intended to be used at design time.</span></span> <span data-ttu-id="0b84f-811">部署的應用程式不應該參考它。</span><span class="sxs-lookup"><span data-stu-id="0b84f-811">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="0b84f-812">將套件設定為 DevelopmentDependency 會加強此建議。</span><span class="sxs-lookup"><span data-stu-id="0b84f-812">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="0b84f-813">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-813">**Mitigations**</span></span>

<span data-ttu-id="0b84f-814">若您必須參考此套件以覆寫 EF Core 的設計階段行為，您可以更新您專案中的 PackageReference 項目中繼資料。</span><span class="sxs-lookup"><span data-stu-id="0b84f-814">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="0b84f-815">若以可轉移方式透過 Microsoft.EntityFrameworkCore.Tools 參考該套件，您將必須新增明確的 PackageReference 到該套件以變更其中繼資料。</span><span class="sxs-lookup"><span data-stu-id="0b84f-815">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="0b84f-816">SQLitePCL.raw 已更新為 2.0.0 版</span><span class="sxs-lookup"><span data-stu-id="0b84f-816">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="0b84f-817">追蹤問題 #14824</span><span class="sxs-lookup"><span data-stu-id="0b84f-817">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="0b84f-818">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-818">**Old behavior**</span></span>

<span data-ttu-id="0b84f-819">Microsoft.EntityFrameworkCore.Sqlite 先前相依於 SQLitePCL.raw 的 1.1.12 版。</span><span class="sxs-lookup"><span data-stu-id="0b84f-819">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="0b84f-820">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-820">**New behavior**</span></span>

<span data-ttu-id="0b84f-821">我們已更新我們的套件以相依於 2.0.0 版。</span><span class="sxs-lookup"><span data-stu-id="0b84f-821">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="0b84f-822">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-822">**Why**</span></span>

<span data-ttu-id="0b84f-823">2\.0.0 版的 SQLitePCL.raw 以 .NET Standard 2.0 為目標。</span><span class="sxs-lookup"><span data-stu-id="0b84f-823">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="0b84f-824">它先前以 .NET Standard 1.1 為目標，這需要大量的大量的可轉移套件才能運作。</span><span class="sxs-lookup"><span data-stu-id="0b84f-824">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="0b84f-825">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-825">**Mitigations**</span></span>

<span data-ttu-id="0b84f-826">SQLitePCL.raw version 2.0.0 包括一些中斷性變更。</span><span class="sxs-lookup"><span data-stu-id="0b84f-826">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="0b84f-827">如需詳細資訊，請參閱[版本資訊](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="0b84f-827">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="0b84f-828">NetTopologySuite 已更新為 2.0.0 版</span><span class="sxs-lookup"><span data-stu-id="0b84f-828">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="0b84f-829">追蹤問題 #14825</span><span class="sxs-lookup"><span data-stu-id="0b84f-829">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="0b84f-830">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-830">**Old behavior**</span></span>

<span data-ttu-id="0b84f-831">空間套件先前相依於 NetTopologySuite 1.15.1 版。</span><span class="sxs-lookup"><span data-stu-id="0b84f-831">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="0b84f-832">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-832">**New behavior**</span></span>

<span data-ttu-id="0b84f-833">我們已更新我們的套件以相依於 2.0.0 版。</span><span class="sxs-lookup"><span data-stu-id="0b84f-833">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="0b84f-834">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-834">**Why**</span></span>

<span data-ttu-id="0b84f-835">NetTopologySuite 2.0.0 版旨在解決 EF Core 使用者遇到的數個可用性問題。</span><span class="sxs-lookup"><span data-stu-id="0b84f-835">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="0b84f-836">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-836">**Mitigations**</span></span>

<span data-ttu-id="0b84f-837">NetTopologySuite 2.0.0 版包括一些中斷性變更。</span><span class="sxs-lookup"><span data-stu-id="0b84f-837">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="0b84f-838">如需詳細資訊，請參閱[版本資訊](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="0b84f-838">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="0b84f-839">必須設定多個不明確的自我參考關聯性</span><span class="sxs-lookup"><span data-stu-id="0b84f-839">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="0b84f-840">追蹤問題 #13573</span><span class="sxs-lookup"><span data-stu-id="0b84f-840">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="0b84f-841">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-841">**Old behavior**</span></span>

<span data-ttu-id="0b84f-842">具有多個自我參考單向導覽屬性和相符 FK 的實體類型，不當設定為單一關聯性。</span><span class="sxs-lookup"><span data-stu-id="0b84f-842">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="0b84f-843">例如:</span><span class="sxs-lookup"><span data-stu-id="0b84f-843">For example:</span></span>

```C#
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

<span data-ttu-id="0b84f-844">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-844">**New behavior**</span></span>

<span data-ttu-id="0b84f-845">這種情況現在會在模型建立過程中偵測到，而且會擲回例外狀況，指出模型不明確。</span><span class="sxs-lookup"><span data-stu-id="0b84f-845">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="0b84f-846">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-846">**Why**</span></span>

<span data-ttu-id="0b84f-847">產生的模型不明確，在這種情況下通常會有錯誤。</span><span class="sxs-lookup"><span data-stu-id="0b84f-847">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="0b84f-848">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-848">**Mitigations**</span></span>

<span data-ttu-id="0b84f-849">使用關聯性的完整設定。</span><span class="sxs-lookup"><span data-stu-id="0b84f-849">Use full configuration of the relationship.</span></span> <span data-ttu-id="0b84f-850">例如:</span><span class="sxs-lookup"><span data-stu-id="0b84f-850">For example:</span></span>

```C#
modelBuilder
     .Entity<User>()
     .HasOne(e => e.CreatedBy)
     .WithMany();
 
 modelBuilder
     .Entity<User>()
     .HasOne(e => e.UpdatedBy)
     .WithMany();
```

<a name="udf-empty-string"></a>
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="0b84f-851">DbFunction。架構為 null 或空字串，將其設定為模型的預設架構</span><span class="sxs-lookup"><span data-stu-id="0b84f-851">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="0b84f-852">追蹤問題 #12757</span><span class="sxs-lookup"><span data-stu-id="0b84f-852">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="0b84f-853">**舊行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-853">**Old behavior**</span></span>

<span data-ttu-id="0b84f-854">以架構為空字串所設定的 DbFunction，在沒有架構的情況下被視為內建函數。</span><span class="sxs-lookup"><span data-stu-id="0b84f-854">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="0b84f-855">例如，下列程式碼會`DatePart`將 CLR 函數`DATEPART`對應至 SqlServer 上的內建函數。</span><span class="sxs-lookup"><span data-stu-id="0b84f-855">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="0b84f-856">**新行為**</span><span class="sxs-lookup"><span data-stu-id="0b84f-856">**New behavior**</span></span>

<span data-ttu-id="0b84f-857">所有的 DbFunction 對應都會被視為對應至使用者定義的函數。</span><span class="sxs-lookup"><span data-stu-id="0b84f-857">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="0b84f-858">因此，空的字串值會將函數放在模型的預設架構內。</span><span class="sxs-lookup"><span data-stu-id="0b84f-858">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="0b84f-859">這可能是透過 Fluent API `modelBuilder.HasDefaultSchema()` `dbo`明確設定的架構，否則為。</span><span class="sxs-lookup"><span data-stu-id="0b84f-859">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="0b84f-860">**原因**</span><span class="sxs-lookup"><span data-stu-id="0b84f-860">**Why**</span></span>

<span data-ttu-id="0b84f-861">先前的架構是空的，這是將該函式內建的方法，但該邏輯僅適用于 SqlServer，其中內建函數不屬於任何架構。</span><span class="sxs-lookup"><span data-stu-id="0b84f-861">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="0b84f-862">**風險降低**</span><span class="sxs-lookup"><span data-stu-id="0b84f-862">**Mitigations**</span></span>

<span data-ttu-id="0b84f-863">手動設定 DbFunction 的轉譯，以將其對應至內建函數。</span><span class="sxs-lookup"><span data-stu-id="0b84f-863">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
